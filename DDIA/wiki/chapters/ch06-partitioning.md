---
title: "Ch 6 — Partitioning"
type: chapter
chapters: [6]
tags: [partitioning, sharding, distributed, rebalancing, routing]
status: solid
updated: 2026-05-16
---

# Ch 6 — Partitioning

## One-paragraph thesis

When data or throughput exceeds what replication alone can handle, **partition**
(shard) it: each record belongs to exactly one partition, each partition is a small
database, partitions spread across a shared-nothing cluster. The goal is to spread
data *and* load **evenly** and avoid **hot spots** (skew). Two schemes —
**key-range** (sorted, good range scans, hot-spot risk) and **hash** (even load,
no range scans) — interact awkwardly with **secondary indexes** (document/local vs.
term/global). Adding/removing nodes requires **rebalancing**, and clients need
**request routing** to find the right partition. *(DDIA Ch 6; partitioning is usually
combined with [[ch05-replication|replication]].)*

## Key ideas

- **[[partitioning-basics]]** — sharding terminology zoo (shard/region/tablet/vnode);
  skew & hot spots; partitioning ⟂ replication (each partition replicated).
- **[[key-range-vs-hash-partitioning]]** — sorted ranges (range scans, hot spots) vs.
  hashed (even, no range scans); Cassandra's compound-key compromise; the
  "consistent hashing" terminology trap.
- **[[skewed-workloads-and-hot-spots]]** — even hashing can't fix a single hot key
  (celebrity); app-level key salting + extra read work.
- **[[local-vs-global-secondary-indexes]]** — document-partitioned (local,
  scatter/gather reads) vs. term-partitioned (global, single-partition reads, slow
  multi-partition writes).
- **[[rebalancing-partitions]]** — *not* `hash mod N`; fixed partition count /
  dynamic / proportional-to-nodes; automatic vs. manual (humans in the loop).
- **[[request-routing]]** — three approaches; ZooKeeper vs. gossip; a consensus /
  service-discovery problem.
- **[[parallel-query-execution]]** — MPP for analytics (deep dive → Ch 10).

## Concepts introduced

- [[partitioning-basics]] · [[key-range-partitioning]] · [[hash-partitioning]]
- [[skewed-workloads-and-hot-spots]] · [[rebalancing-partitions]] ·
  [[request-routing]] · [[parallel-query-execution]]

## Comparisons introduced

- [[key-range-vs-hash-partitioning]] · [[local-vs-global-secondary-indexes]]

## Systems / papers referenced

- [[cassandra]] (vnodes, compound key, gossip, proportional rebalancing),
  [[zookeeper]] (partition→node metadata), HBase/RethinkDB (dynamic),
  [[mongodb]], [[voldemort]], Couchbase, Espresso/Helix

## Trade-offs & tensions

- Range scans & sorted access (key-range) vs. even load (hash) — Cassandra compound
  key is the hybrid.
- Local index: cheap writes, scatter/gather reads (tail-latency amplification —
  [[response-time-percentiles]]) vs. global index: single-partition reads, costly
  cross-partition (often async) writes.
- Automatic rebalancing convenience vs. unpredictability (auto-rebalance +
  auto-failure-detection → cascading failure → humans in the loop).
- `hash mod N` is simple but moves almost everything when N changes.

## Connections to other chapters

- Partitioning ⟂ [[ch05-replication]]; per-partition leader/follower.
- Cross-partition writes (one succeeds, one fails) & distributed-transaction
  consistency for global indexes → [[ch07-transactions]],
  [[ch09-consistency-and-consensus]].
- Routing/consensus & ZooKeeper → [[ch09-consistency-and-consensus]]; consistent
  prefix / causality → [[consistent-prefix-reads]].
- MPP parallel query → [[ch10-batch-processing]]; term-partitioned indexes revisited
  → [[ch12-the-future-of-data-systems]].

## Open questions / things to revisit

- How do distributed transactions make global secondary indexes consistent (Ch 7/9)?
- ZooKeeper's role across the book (routing here, locks/consensus Ch 9).
