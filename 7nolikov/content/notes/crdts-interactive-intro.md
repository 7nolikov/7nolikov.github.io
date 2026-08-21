---
title: An interactive intro to CRDTs
date: 2026-08-21
categories: [architecture]
---

CRDTs explained with widgets you can drag around, which is the only way this subject has ever made sense to me.

The core idea: design your data type so that merges are commutative, associative and idempotent. Then two replicas that saw the same edits in different orders end up identical, and you do not need a server to arbitrate.

You build up from a counter to a set to collaborative text, and each step shows why the naive version breaks first.

[An Interactive Intro to CRDTs](https://jakelazaroff.com/words/an-interactive-intro-to-crdts/)

See also:

- {{< backlink "uncloud" >}}
- {{< backlink "saga-pattern" >}}
