---
title: TIGER_STYLE - TigerBeetle's engineering doc
date: 2026-08-21
categories: [architecture]
---

The engineering style guide from TigerBeetle, a distributed financial database. It is not really a style guide - it's a document about how to build software that must not lose data.

Things in it I keep coming back to: assert everything, at every layer, in production too. Allocate all memory up front so you can't fail at the worst moment. Set an explicit limit on every loop and every queue, because "unbounded" is a bug waiting for load. Design so a failure is loud rather than silent.

You will not adopt all of it - most of us don't write databases. But the underlying idea transfers to anything: decide what must never happen, then make the system say so out loud instead of hoping.

[TIGER_STYLE.md](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/TIGER_STYLE.md)

See also:

- {{< backlink "saga-pattern" >}}
- {{< backlink "reproducible-builds" >}}
