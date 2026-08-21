---
title: Reproducible Builds
date: 2026-08-21
categories: [ops]
---

A set of practices for making a build produce a bit-for-bit identical artifact every time from the same source.

The reason it matters is supply chain: if the binary you ship cannot be reproduced from the source you published, nobody - including you - can verify that the two correspond. Reproducibility turns "trust the build server" into something checkable by anyone.

The things that break it are mundane and everywhere: embedded timestamps, absolute paths, locale, filesystem ordering, and anything that hashes a map iterated in random order. The site catalogues each one and how to fix it.

[reproducible-builds.org](https://reproducible-builds.org/)

See also:

- {{< backlink "distroless" >}}
- {{< backlink "conventional-commits" >}}
