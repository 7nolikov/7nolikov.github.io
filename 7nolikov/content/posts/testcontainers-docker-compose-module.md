---
title: My plan said Testcontainers. The code never imported it.
date: 2026-07-30
categories: [testing]
---

The build plan for my last project allocated 90 minutes to a task called "Integration tests
with testcontainers". It sketched the directory layout. It named the file.

The library was never installed. Not once, in the whole repository.

<!--more-->

## What actually got built

Integration tests exist. They run against a real Postgres, a real object store and a real
Kafka broker. They just don't use Testcontainers to get them.

They use a build tag:

```go
//go:build integration

// Integration tests for the job repository against a real PostgreSQL instance.
// They exercise the SQL that unit tests with fakes cannot reach: partition scoping,
// the dedup status filter, and the reconciler's staleness query.
//
// Run with:
//
//	TEST_DB_DSN="postgres://..." go test -tags integration ./internal/job/...
```

The infrastructure comes from the Docker Compose stack that was already there for local
development. `make up` starts it. The tests connect over environment variables. Without the
tag they don't compile, so a normal `go test ./...` stays fast and needs nothing running.

The runner has tiers, which is the part I'd keep in any project:

```
fast    unit + lint                 no infrastructure needed   [default]
infra   fast + integration + e2e    needs `make up`
full    infra + demo                needs `make up`
```

## Why not the library

Honestly? Because the Compose stack already existed.

I had written it for local development before any test needed it. Testcontainers would have
started a second set of containers, per package, with a lifecycle I'd have to manage,
duplicating infrastructure that was already running on my machine. The tests would have been
more isolated and slower, and I'd have maintained two definitions of the same environment.

So the choice made itself. What I never did was go back and update the plan.

## The part that's actually embarrassing

Five separate documents said the project used `testcontainers-go`. The execution guide, a
directory listing, a testing section, a task estimate, an architecture note.

None of it was true. It had never been true.

I found it by grepping for the import, months later, during an audit where I was checking
documented claims against the code. Nothing failed. Nothing warned. The docs described a
plausible project that was not the one in the repository.

That is the normal way this happens, by the way. Nobody writes a false claim on purpose. You
write a plan, the plan is reasonable, the implementation diverges for a good reason, and the
plan quietly becomes fiction because updating it isn't anybody's task.

## Both approaches, honestly

**Testcontainers** owns the container lifecycle from inside the test. Containers start and
stop with the test run, get random ports, and leave nothing behind. Nobody has to remember to
start anything. CI is trivial - if Docker is available, the test works. The cost is startup
time on every run and a second description of your environment living in test code.

**Compose plus a build tag** reuses the environment you already run by hand. Iteration is
fast, because the stack is already warm. The definition lives in one place. The cost is
shared state between tests, a dependency on somebody having run `make up`, and a real risk
that CI is configured differently from your laptop.

If you have no local stack, use Testcontainers. If you already have one and it's the same
stack you deploy, the build tag is less machinery.

Neither of these is the interesting decision.

## What mattered was neither

Here is the actual finding from that audit, and it has nothing to do with which library I
picked.

CI had no database at all. So the repository layer - partition scoping, deduplication, record
versioning, the unique index that drives a retry path - had **zero** test coverage. Not thin.
None. There was nowhere to run such a test even if I'd written one.

I added Postgres to CI and wrote the tests. The first run, every single one failed before
reaching an assertion. Not because the tests were wrong - because a migration could not
upgrade a database that already had rows in it. The service would not have started on any
existing deployment.

That bug was reachable only by running real code against a real database. Testcontainers
would have found it. Compose plus a build tag found it. A mock would not have found it, and
neither would any amount of arguing about which of the two to use.

The tooling debate is a rounding error. The question is whether your tests ever touch real
infrastructure at all.

## The rule I took away

Pick whichever gets a real database in front of your code with the least friction, then spend
the saved time making sure it runs in CI. An integration test that only runs on your laptop
is a personal hobby.

And go grep your own docs for a library name. See if you actually import it.

See also:

- {{< backlink "hurl" >}}
- {{< backlink "go-synctest" >}}
- {{< backlink "learn-go-with-tests" >}}
