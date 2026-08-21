---
title: Everything I shipped but never ran was broken
date: 2026-08-21
categories: [go, observability]
---

I had a free day, so I made coffee and sat down to read my own documentation against the code. I hate errors in documentation. I expected to fix a few stale sentences and move on.

<!--more-->

The first thing my eye caught was a table of performance results:

| Metric | Before | After |
|--------|--------|-------|
| p50 API latency | 361 ms | ~15 ms |
| p99 API latency | 5,348 ms | ~100 ms |
| Throughput | ~8 req/s | ~300 req/s |

With improvement ratios: 24×, 53×, 37×.

The actual benchmark, in the README, in the same repository, says **97 ms p50 and 38.1 req/s**. The "results" were 5–8× optimistic.

Nobody lied. Those numbers started life as design targets in a planning document, got copied into a table, and the table grew a heading that said what the numbers had become rather than what they were. The async worker pool I built to hit `~15 ms` did help — p99 improved roughly tenfold — it just never got close to the projection, because reading the payload, hashing it, and a database insert all still sit in the request path.

I rewrote every performance claim as projected-versus-measured, side by side, and labelled every unmeasured environment as an estimate.

**Documentation drift has a direction.** Not one stale claim I found was pessimistic. Nobody accidentally under-sells their own project. If you audit in only one direction, audit that one.

That was the boring part of the day. By the end of it I had 23 defects written down — two of them release-blocking, and neither of those was visible from reading the code.

Then I ran a load test.

## The bug that was eating records

Three hundred records, five concurrent workers — a load the pipeline had handled before. The ingest service didn't slow down. It died.

```
OOMKilled: true
ExitCode: 137
```

Two hundred and thirty-nine records were gone. Not failed, not dead-lettered — gone, with no record of them anywhere except a row in Postgres that said `ACCEPTED` and would never change. And because I had never set a restart policy, the service stayed down. Ingestion had simply stopped.

My first thought was not technical. It was: how did I manage to do this to myself.

Here is the entire bug:

```go
p.client.PutObject(ctx, p.bucket, key, reader, -1, putOpts)
```

That `-1` means "I don't know how long this stream is." For genuinely streaming data it's the right answer. But a client told the length is unknown cannot know where multipart boundaries fall, so it does the only safe thing: it allocates a staging buffer — 16 MiB minimum — **for every concurrent upload**.

The ingest service runs a pool of twenty upload workers. Twenty simultaneous PUTs of roughly one-kilobyte records were reserving hundreds of megabytes of buffers. The store service did the same on its own write path, which is why it sat at **1.07 GiB of resident memory while essentially idle** — a number I had looked at several times without registering as strange.

Both callers already had the payload in memory as a byte slice. The length was free. It was simply never passed.

Same load test, after:

| | Before | After |
|---|--------|-------|
| 300 records, concurrency 5 | OOM-killed; 239 lost | **300/300 completed in 20 s** |
| store service memory | 1.07 GiB | **17 MiB** |

The part I couldn't fix is more interesting. Those 239 records were unrecoverable, and no retry logic would have changed that. Between returning `202 Accepted` and completing the staging upload, a record exists in exactly one place: the ingest process's memory. A `SIGKILL` in that window destroys it.

What the system did do was tell me precisely that. The reconciler noticed jobs stranded in `ACCEPTED`, checked staging, and failed them with a diagnosis instead of retrying into the void:

```
ingest incomplete: staging object "raw/document/6375743d-.../record.json"
missing 5m0s after acceptance; payload lost before staging
```

That window is the real durability boundary of an async accept path. It's a legitimate tradeoff — it's how you return `202` in double-digit milliseconds — but it needs stating out loud, because "accepted" reads like "durable" and here it isn't.

Worth noting how old this bug was. The project's first commit is from February. The OOM was found at the end of July. That `-1` had been sitting there for months, under every feature I added on top of it.

## Writing the first test found a release blocker

The job and catalog repositories — partition scoping, deduplication, record versioning — had zero test coverage. Not thin coverage. None. And CI had no database to run any against.

I added Postgres to CI and wrote integration tests against a real database. On the first run, every catalog test failed before reaching a single assertion:

```
migrate catalog table: "CREATE UNIQUE INDEX IF NOT EXISTS
catalog_partition_record_version_idx ON catalog (partition, record_id, version)":
ERROR: could not create unique index (SQLSTATE 23505)
```

Here's the migration:

```sql
ALTER TABLE catalog ADD COLUMN IF NOT EXISTS record_id TEXT NOT NULL DEFAULT '';
ALTER TABLE catalog ADD COLUMN IF NOT EXISTS version  BIGINT NOT NULL DEFAULT 1;
CREATE UNIQUE INDEX IF NOT EXISTS catalog_partition_record_version_idx
  ON catalog (partition, record_id, version);
```

Rows written before record versioning existed have no record id. `ADD COLUMN ... DEFAULT` gives every one of them the identical key `('', '', 1)`. You cannot build a unique index over that. `Migrate` returns an error, and `main` exits on it.

Which means the store service would not start on any database upgraded from the previous release. I checked the live database: five legacy rows collapsed onto one key, and the unique index was simply absent.

The failure mode that didn't happen is worse. Suppose that error had been logged and swallowed instead of fatal. The service starts. The index doesn't exist. And the version-conflict detection the entire store saga depends on is silently gone — concurrent writers stop being detected, and nothing reports a problem.

The fix is one line, before the index:

```sql
UPDATE catalog SET record_id = job_id WHERE record_id = '';
```

Why had I never hit this? Because the containers I'd been running were built before versioning existed. They migrated their own older schema perfectly happily. Reproducing it requires current code against an older database — testing the upgrade, not the install.

## Three features that were never plugged in

The tracing decorator had no callers. A storage wrapper that emits a span per operation, implemented, documented, drawn in the architecture diagram — and never wired into anything. All three services constructed their storage clients directly. In a project whose whole thesis is *observability is the product*, object storage was the one hop nobody could see. Wiring it in took four lines and moved a complete trace from 14 spans to 21.

Topic partitioning was inert. The Compose file provisions the Kafka topics with three partitions, with a comment explaining that this is what lets consumers scale. The live topics had one partition — the topic-creation command succeeds silently on a topic that already exists, so the setting only ever applied to a brand-new volume. A consumer group can't use more consumers than partitions, so every extra replica would have sat idle, and nothing would have reported anything wrong.

And scaling didn't work at all:

```
Bind for 0.0.0.0:9092 failed: port is already allocated
```

Fixed host ports, so a second replica could never start. The horizontal scaling described in that same file's own comment was unreachable.

After fixing both, I could finally measure the claim I'd been making for months: 16.5 rec/s on one replica each, 50.3 rec/s at three — 3.05×, linear, no code changes. The claim was true. It had just never once been executed.

## What I learned

Read the code - found stale docs. Running the system - found very serious defects.

Not even one of them required deep insight. Each was a feature I had written, documented, reasonably believed worked. I just postponed execution in the way a real user would do it - an existing volume, an existing database, more than one replica, a process that gets killed rather than asked kindly to stop.

So a feature isn't done when the code is written and some tests pass. It must be verified against real world scenarios. For anything with persistent state that means testing the upgrade, not just the install. My worst bug was reachable only by running new code against an old database. It's quite common for every real deployment.

The irony I can't dodge: this is a project about observability, and none of the tools I made caught any of this. Traces, metrics, dashboards, SLOs tell you about records flowing through a system that is running. They are silent about the capability you shipped that was never plugged in, or the migration that will fail on somebody else's database next sprint.

How do you plan and test the upgrade path in your own projects? I still don't have a good answer beyond "keep an old database backup around."

See also:

- {{< backlink "tiger-style" >}}
- {{< backlink "kubernetes-failure-stories" >}}
- {{< backlink "saga-pattern" >}}
