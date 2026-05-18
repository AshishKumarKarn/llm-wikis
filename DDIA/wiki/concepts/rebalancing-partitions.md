---
title: Rebalancing Partitions
type: concept
chapters: [6]
tags: [partitioning, rebalancing, operations]
status: solid
updated: 2026-05-16
---

# Rebalancing Partitions

## Definition

Moving load (data + requests) from one node to another when throughput grows, data
grows, or a machine fails. Requirements: fair load afterward, DB stays available
during, move no more data than necessary. *(DDIA Ch 6)*

## Strategies

- **`hash mod N` — how *not* to do it.** Changing N moves almost every key (123456:
  node 6 at N=10, node 3 at N=11, node 0 at N=12). Excessively expensive.
- **Fixed number of partitions.** Create *many more* partitions than nodes (e.g.
  1000 partitions / 10 nodes); a new node steals whole partitions from each existing
  node. Partition count fixed at setup (≈ max future nodes); only assignment changes.
  Riak, ES, Couchbase, Voldemort. Hard to size if dataset size varies a lot
  (partition size ∝ data).
- **Dynamic partitioning.** Split a partition when it exceeds a size (HBase 10 GB),
  merge when too small (like a B-tree top level). Count adapts to data volume.
  Caveat: empty DB = one partition (one node busy) → **pre-splitting**. Works for
  key-range *and* hash (MongoDB ≥2.4).
- **Proportional to nodes.** Fixed partitions *per node* (Cassandra 256/node,
  Ketama); a new node splits random existing partitions and takes half of each.
  Corresponds to the original consistent-hashing idea; needs hash-based boundaries.

## Automatic vs. manual

A gradient. Fully automatic = less ops work but **unpredictable**: rebalancing is
expensive (reroute + move data) and, combined with **automatic failure detection**,
dangerous — an overloaded (not dead) node gets declared dead, load moves off it,
worsening things → **cascading failure**. A **human in the loop** (Couchbase/Riak/
Voldemort suggest, admin commits) prevents operational surprises.

## Related concepts

- [[partitioning-basics]] · [[key-range-partitioning]] · [[hash-partitioning]] ·
  [[request-routing]]
- [[failover-and-split-brain]] — the auto-rebalance + auto-failure-detection hazard

## Sources

DDIA Ch 6 ("Rebalancing Partitions").
