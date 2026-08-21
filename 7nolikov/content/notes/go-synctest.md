---
title: Go's synctest - deterministic concurrency tests
date: 2026-08-21
categories: [golang]
---

`synctest` gives concurrent Go tests a fake clock and a way to wait until every goroutine in a bubble is blocked.

This kills the worst pattern in Go testing: `time.Sleep(100 * time.Millisecond)` scattered through tests to "let things settle". Those sleeps make suites slow and flaky at the same time - too short on a loaded CI runner, too long everywhere else.

With synctest a one-hour timeout test runs instantly and deterministically, because the clock only moves when nothing is left to do.

[Go's synctest is amazing](https://oblique.security/blog/go-synctest/)

See also:

- {{< backlink "learn-go-with-tests" >}}
- {{< backlink "hurl" >}}
