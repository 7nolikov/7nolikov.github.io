---
title: Kubernetes Failure Stories
date: 2026-08-21
categories: [kubernetes]
---

A collected list of public post-mortems from companies running Kubernetes in production. Real outages, written up by the people who were on call.

It is the best argument I know against learning a platform only from its documentation. The docs tell you what the system does. These tell you what it does at 3am when a node's disk fills up, a CNI plugin misbehaves, or a `livenessProbe` restarts a healthy pod into an outage.

Recurring theme worth noticing: very few of these are exotic. Most are a default nobody changed, a limit nobody set, or a config that silently meant something other than what the author assumed.

[k8s.af](https://k8s.af/)

See also:

- {{< backlink "architecture-antipatterns" >}}
- {{< backlink "distroless" >}}
