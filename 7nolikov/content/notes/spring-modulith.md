---
title: Spring Modulith 1.3
date: 2024-12-23
categories: [java]
---

Microservices -> modulith: the trend became real.

Spring Modulith supports modular monoliths - clear boundaries between modules, verified at build time, all running in one process. You get the enforced separation that pushed people to microservices, without the network between the parts.

Module boundaries are checked by tests, so a violation fails the build instead of being noticed a year later. If you ever need to split a module into a service, the seam is already there.

[What's new in Spring Modulith 1.3](https://spring.io/blog/2024/11/22/whats-new-in-spring-modulith-1-3) (2024 - later releases have shipped since)
