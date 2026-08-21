---
title: System Design Primer
date: 2026-08-21
categories: [architecture]
---

The open-source collection people actually mean when they say "go read the system design primer". Caching, sharding, replication, load balancing, CAP, back-of-envelope numbers, then worked examples of designing well-known systems.

The part I would not skip is the latency numbers table - memory reference vs SSD read vs a round trip between datacenters, in nanoseconds. Once those orders of magnitude are in your head, a lot of design arguments resolve themselves before anybody draws a box.

Free, and thorough enough that most paid courses are a repackaging of it.

[github.com/donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer)

See also:

- {{< backlink "c10k-problem" >}}
- {{< backlink "software-architecture-premier" >}}
