---
title: NATS
date: 2026-08-21
categories: [architecture]
---

A messaging system that covers request/reply, pub/sub and queue groups in one small server, with JetStream adding persistence and streams when you need them.

Where it differs from Kafka: NATS core is fire-and-forget and extremely light - a single binary, microsecond latencies, no ZooKeeper-era operational weight. Kafka is a durable, replayable log first, and everything else second.

So the question is not which is better, it's whether you need a log you can replay from an arbitrary offset months later. If you do, use a log. If you are moving messages between services right now, NATS is a fraction of the operational cost.

[nats.io](https://nats.io/)

See also:

- {{< backlink "saga-pattern" >}}
- {{< backlink "c10k-problem" >}}
