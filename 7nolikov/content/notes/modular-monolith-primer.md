---
title: "Modular Monolith: a primer"
date: 2026-08-21
categories: [architecture]
---

Kamil Grzybek on the middle option between a big ball of mud and a fleet of services.

The argument that lands: microservices give you module boundaries by making them physical - you cannot accidentally call into another module's database over HTTP without noticing. But you pay for that with a network between every part of your system, and you pay in latency, partial failure and deployment machinery.

A modular monolith tries to get the boundaries without the network, which means the boundaries have to be enforced some other way - by tests, by build rules, by discipline. That is the whole difficulty, and this piece is honest about it.

[Modular Monolith: A Primer](https://www.kamilgrzybek.com/blog/posts/modular-monolith-primer)

See also:

- {{< backlink "spring-modulith" >}}
- {{< backlink "architecture-antipatterns" >}}
