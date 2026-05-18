---
title: Partitioning Basics (Skew & Hot Spots)
type: concept
chapters: [6]
tags: [partitioning, sharding, distributed]
status: solid
updated: 2026-05-16
---

# Partitioning Basics (Skew & Hot Spots)

## Definition

**Partitioning** (a.k.a. sharding) breaks a large dataset so each record belongs to
**exactly one partition**; each partition is effectively a small database. Spreads
data across disks and query load across processors in a shared-nothing cluster. *(DDIA
Ch 6)*

> Terminology zoo: *shard* (MongoDB/ES/Solr), *region* (HBase), *tablet* (Bigtable),
> *vnode* (Cassandra/Riak), *vBucket* (Couchbase). DDIA uses **partition**.

## Why it matters

The main reason is **scalability** beyond what [[ch05-replication|replication]] alone
gives. Single-partition queries scale by adding nodes; complex queries can
parallelize (harder). Pioneered by Teradata/Tandem in the 1980s, rediscovered by
NoSQL & Hadoop warehouses.

## Skew & hot spots

Goal: spread data **and** load evenly (10 nodes → ~10× capacity). **Skew** =
uneven partitioning; a **hot spot** = a partition with disproportionate load (worst
case: all load on one node, the rest idle). Random assignment avoids hot spots but
loses the ability to find a record without querying every node — so we use
structured schemes ([[key-range-partitioning]], [[hash-partitioning]]).

## Partitioning ⟂ replication

Independent choices, usually combined: each partition is replicated to several nodes.
With leader/follower, a node may be leader for some partitions and follower for
others. Everything in [[ch05-replication]] applies to partition replicas.

## Related concepts

- [[key-range-partitioning]] · [[hash-partitioning]] ·
  [[skewed-workloads-and-hot-spots]]
- [[replication-vs-partitioning]] · [[shared-nothing-architecture]]

## Sources

DDIA Ch 6 (intro, "Partitioning and Replication", "Partitioning of Key-Value Data").
