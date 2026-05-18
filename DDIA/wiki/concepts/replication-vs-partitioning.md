---
title: Replication vs. Partitioning
type: concept
chapters: [4, 5, 6]
tags: [distributed, replication, partitioning, part-ii]
status: developing
updated: 2026-05-16
---

# Replication vs. Partitioning

## Definition

The two ways data is distributed across nodes (the Part II framing). *(DDIA Part II
intro)*

- **Replication** — keep a *copy of the same data* on several nodes (possibly in
  different locations). Provides redundancy (serve from survivors) and can improve
  performance. → [[ch05-replication]].
- **Partitioning (sharding)** — split a big database into *disjoint subsets*
  (partitions) assigned to different nodes. → [[ch06-partitioning]].

## Why it matters

Separate mechanisms but **usually combined** (e.g. two partitions, two replicas
each). Together they are the foundation of every distributed datastore in Part II;
the trade-offs they introduce (and that transactions, Ch 7, and the distributed-
systems troubles, Ch 8–9, address) are the subject of the rest of Part II.

## Reasons to distribute at all

Scalability (load > one machine), fault-tolerance/HA (survive node/datacenter loss),
latency (serve users from a nearby region). The book focuses on
[[shared-memory-vs-shared-disk-vs-shared-nothing|shared-nothing]] because it demands
the most caution — the database can't hide its trade-offs from you.

## Related concepts

- [[ch05-replication]] · [[ch06-partitioning]]
- [[shared-memory-vs-shared-disk-vs-shared-nothing]] ·
  [[shared-nothing-architecture]]
- [[ch07-transactions]] · [[ch08-the-trouble-with-distributed-systems]]

## Sources

DDIA Part II introduction ("Distributed Data").
