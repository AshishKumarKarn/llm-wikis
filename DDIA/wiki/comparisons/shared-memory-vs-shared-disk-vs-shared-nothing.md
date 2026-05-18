---
title: "Shared-Memory vs. Shared-Disk vs. Shared-Nothing"
type: comparison
chapters: [4]
tags: [distributed, architecture, scalability, part-ii]
status: solid
updated: 2026-05-16
---

# Shared-Memory vs. Shared-Disk vs. Shared-Nothing

The three distributed-data architectures (Part II intro). *(DDIA)*

| | Shared-memory | Shared-disk | Shared-nothing |
|---|---|---|---|
| AKA | vertical / scale up | — | horizontal / scale out |
| Setup | Many CPUs/RAM/disks, one OS, fast interconnect | Independent CPU/RAM, shared disk array over fast network | Independent nodes; coordinate only in software over a conventional network |
| Cost | Grows **superlinearly**; 2× size ≯ 2× load | Moderate | Best price/performance; commodity HW |
| Geo | Single location | Single location | Multi-region possible |
| Fault tolerance | Limited (hot-swap parts) | Some | Survive node/datacenter loss |
| Limits | Bottlenecks, cost | Lock/contention overhead limits scalability (some warehousing) | App complexity; data-model expressiveness limits |

## The book's stance

Part II focuses on **shared-nothing** — not because it's always best (a single-
threaded program can beat a 100-core cluster; "Scalability! But at what COST?"), but
because it **requires the most caution from the application developer**: the database
cannot magically hide distributed trade-offs. Once data spans nodes you must reason
about [[replication-vs-partitioning|replication and partitioning]] and the troubles
of [[ch08-the-trouble-with-distributed-systems|distributed systems]].

## Related

- [[shared-nothing-architecture]] (the Ch 1 scale-up/out view) ·
  [[replication-vs-partitioning]]
- [[scalability]] · [[ch05-replication]] · [[ch06-partitioning]]

## Sources

DDIA Part II intro. Refs: Stonebraker "The Case for Shared Nothing" (1986);
McSherry et al. "Scalability! But at What COST?" (2015).
