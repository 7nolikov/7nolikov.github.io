---
title: Saga pattern
date: 2026-08-21
categories: [architecture]
---

How to keep data consistent across services when you cannot hold a transaction across them.

The idea: break the operation into local transactions, and give each one a compensating action that undoes it. If step three fails, you run the undo for two and one. There is no rollback - there is only doing something else that cancels out the effect.

The part people underestimate is that compensation can fail too. Deleting the object you just wrote can return an error, and then you have an orphan and need to notice it. Any real implementation needs a metric on that path and something that reconciles what got stranded.

[microservices.io - Saga](https://microservices.io/patterns/data/saga.html)

See also:

- {{< backlink "tiger-style" >}}
- {{< backlink "nats" >}}
- {{< backlink "architecture-antipatterns" >}}
