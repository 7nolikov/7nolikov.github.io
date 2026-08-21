---
title: Uncloud - Compose for production
date: 2026-08-21
categories: [ops]
---

Fills the gap between "docker compose on a VPS" and Kubernetes. Clustering, automatic HTTPS, service discovery, a WireGuard mesh between machines - without a control plane.

The design choice I find interesting: there is no central control plane at all. Every machine holds the full cluster state in a CRDT-backed distributed SQLite, syncing peer to peer. No etcd, no leader election, and the cluster keeps working when machines go offline.

Aimed at people running a $5 VPS, a spare Mac mini, or a couple of bare-metal boxes who do not need Google-scale and should not pay Google-scale complexity for it. Written in Go.

[uncloud.run](https://uncloud.run/)
