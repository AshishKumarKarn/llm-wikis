---
title: Apache Cassandra
type: system
chapters: [2, 3, 5, 6]
tags: [system, nosql, column-family, leaderless, partitioning]
status: developing
updated: 2026-05-16
---

# Apache Cassandra

Wide-column store using the **Bigtable column-family** data model (shared with
HBase) — introduced in DDIA Ch 2 as another way to manage [[data-locality]] outside
the document model.

Ch 5: Dynamo-style **[[leaderless-replication]]** with tunable
[[quorum-consistency|quorums]]; **LWW is its only conflict-resolution method** (use
unique/UUID keys, treat values immutable — see [[happens-before-and-concurrency]]);
**sloppy quorums off by default**; multi-datacenter within the leaderless model
(n spans DCs, client waits for local-DC quorum). LSM storage with both size-tiered
and leveled compaction ([[sstables-and-lsm-trees]]).

Ch 6: **[[hash-partitioning]]** with a **compound primary key** (hash first column,
concatenated sort index on the rest — the key-range/hash hybrid); **vnodes** (256
partitions/node by default), **proportional-to-nodes** [[rebalancing-partitions]]
(3.0 added a fairer algorithm); **gossip protocol** for cluster state instead of
ZooKeeper ([[request-routing]] approach 1); document-partitioned (local) secondary
indexes.

> `status: developing` — broad coverage; revisit if later chapters add detail.

## Related

- [[leaderless-replication]] · [[quorum-consistency]] · [[hash-partitioning]] ·
  [[rebalancing-partitions]] · [[request-routing]] · [[dynamo-paper]] ·
  [[bigtable-paper]]
