---
title: "Single-Leader vs. Multi-Leader vs. Leaderless Replication"
type: comparison
chapters: [5]
tags: [replication, comparison, distributed]
status: solid
updated: 2026-05-16
---

# Single-Leader vs. Multi-Leader vs. Leaderless Replication

The three replication approaches of [[ch05-replication]]. Almost every distributed
database uses one. *(DDIA Ch 5)*

| | [[single-leader-replication\|Single-leader]] | [[multi-leader-replication\|Multi-leader]] | [[leaderless-replication\|Leaderless]] |
|---|---|---|---|
| Who accepts writes | One leader | Several leaders | Any replica |
| Write ordering | Leader defines it | No global order | No global order |
| Conflicts | None | Yes → [[write-conflict-resolution]] | Yes (even with strict quorums) |
| Failover | Needed (fraught — [[failover-and-split-brain]]) | DCs continue independently | None (quorums handle it) |
| Consistency | Strong-ish on leader; followers lag | Weak | Weak ([[quorum-consistency]]) |
| Fault/latency robustness | Lower (write SPOF) | Higher | Higher |
| Ease of reasoning | Easiest | Hard | Hard |
| Examples | PostgreSQL, MySQL, MongoDB, Kafka | Tungsten/BDR, CouchDB, Google Docs | [[dynamo-paper\|Dynamo]], Cassandra, Riak, Voldemort |

## The trade-off in one line

Single-leader is **easy** (no conflict resolution); multi-leader and leaderless are
**more robust under faults/latency** but harder to reason about and give only **weak
consistency** + conflict handling. Choose by whether you need multi-datacenter
write availability / offline operation (multi/leaderless) or simplicity & strong
single-copy consistency (single-leader).

## Related

- [[single-leader-replication]] · [[multi-leader-replication]] ·
  [[leaderless-replication]] · [[quorum-consistency]] · [[replication-lag]]

## Sources

DDIA Ch 5 (Summary).
