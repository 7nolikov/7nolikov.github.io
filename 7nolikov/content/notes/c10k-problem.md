---
title: The C10K problem
date: 2026-08-21
categories: [architecture]
---

Dan Kegel's 1999 write-up of why a server could not handle ten thousand simultaneous connections, and what would have to change.

It is a historical document now - ten thousand connections is unremarkable - but it is the clearest explanation of *why* every modern runtime is built the way it is. Thread-per-connection does not scale because threads cost memory and context switches. That single constraint produced epoll, kqueue, async I/O, event loops, and eventually goroutines and virtual threads.

Read it and a lot of "why is it like this" questions about Node, Go and Netty answer themselves.

[The C10K problem](https://www.kegel.com/c10k.html)

See also:

- {{< backlink "nats" >}}
- {{< backlink "system-design-primer" >}}
