---
title: Multi-Leader Replication
type: concept
chapters: [5]
tags: [replication, distributed, multi-datacenter, conflicts]
status: solid
updated: 2026-05-16
---

# Multi-Leader Replication

## Definition

(a.k.a. master–master, active/active) More than one node accepts writes; each leader
is also a follower of the others, forwarding its writes to all. Removes the
single-leader write bottleneck/SPOF. *(DDIA Ch 5)*

## Use cases (rarely worth it within one datacenter)

- **Multi-datacenter**: a leader per datacenter; writes processed locally, replicated
  async across DCs. Better write performance (no cross-internet hop), DC-outage and
  network-fault tolerance vs. single-leader. Tools: Tungsten (MySQL), BDR
  (Postgres), GoldenGate (Oracle).
- **Offline clients**: each device is a "leader" with a local DB; sync is async
  multi-leader with lag of hours/days (e.g. calendar apps; CouchDB).
- **Collaborative editing** (Google Docs/Etherpad): small change units, no locking →
  same challenges as multi-leader, needs conflict resolution.

## The big downside

Concurrent writes to the same data on different leaders → **write conflicts** →
[[write-conflict-resolution]]. A "retrofitted" feature in many DBs with subtle
pitfalls (autoincrement, triggers, constraints) — often called "dangerous territory."

## Replication topologies

How writes propagate: **all-to-all** (every leader → every other; best fault
tolerance via multiple paths, but messages can overtake → causality issues like
[[consistent-prefix-reads]], needing version vectors), **circular**, **star**
(forward through nodes, tag with node IDs to stop loops; one node failure breaks the
flow). Many systems implement conflict ordering poorly — test thoroughly.

## Related concepts

- [[write-conflict-resolution]] · [[single-leader-replication]] ·
  [[leaderless-replication]] · [[single-vs-multi-vs-leaderless-replication]]
- [[happens-before-and-concurrency]] · [[consistent-prefix-reads]]

## Sources

DDIA Ch 5 ("Multi-Leader Replication").
