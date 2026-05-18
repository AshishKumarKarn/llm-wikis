---
title: "The Chubby Lock Service (Burrows, 2006)"
type: paper
chapters: [9]
tags: [paper, distributed, coordination, locking]
status: stub
updated: 2026-05-16
---

# The Chubby Lock Service for Loosely-Coupled Distributed Systems

Mike Burrows, 7th USENIX OSDI, November 2006.

Google's distributed lock/coordination service — the model **[[zookeeper]] is based
on**. Combines consensus-backed linearizable operations with a lock/lease API and
small replicated metadata store. See [[coordination-services]]. *(DDIA Ch 9)*

> `status: stub` — anchor for [[coordination-services]].

## Related

- [[coordination-services]] · [[consensus]] · [[zookeeper]]
