---
title: Distroless container images
date: 2026-08-21
categories: [ops]
---

Base images containing your application and its runtime dependencies, and nothing else. No shell, no package manager, no utilities.

The trade-off against Alpine, which is the usual alternative:

**Distroless** has the smaller attack surface - with no shell in the image, a lot of privilege escalation and post-exploitation techniques simply have nothing to run. The static variant is around 2MB. The cost is debugging: you cannot exec in and look around, because there is nothing to exec. In Kubernetes you attach an ephemeral container instead.

**Alpine** is about 5-6MB and comes with BusyBox and `apk`, so it behaves like a Linux box you recognise. Easier to work in, and that same shell and package manager are what distroless removes on purpose.

Both want a multi-stage build. Pick based on whether you would rather debug easily or reduce what an attacker can use.

[GoogleContainerTools/distroless](https://github.com/GoogleContainerTools/distroless)

See also:

- {{< backlink "reproducible-builds" >}}
- {{< backlink "dokku" >}}
