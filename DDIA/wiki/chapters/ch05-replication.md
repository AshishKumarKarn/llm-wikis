---
title: "Ch 5 — Replication"
type: chapter
chapters: [5]
tags: [replication, distributed, consistency, quorum, conflicts]
status: solid
updated: 2026-05-16
---

# Ch 5 — Replication

## One-paragraph thesis

Replication = keeping a copy of the same data on multiple networked machines, for
**high availability**, **disconnected operation**, **latency** (data near users), and
**read scalability**. Copying static data is trivial; *all* the difficulty is
handling **changes**. Almost every distributed database uses one of three approaches
— **single-leader**, **multi-leader**, **leaderless** — each with sharp trade-offs
around consistency, conflicts, and fault tolerance. Asynchronous replication is fast
but introduces **replication lag** and weak consistency; the chapter develops precise
consistency models (read-after-write, monotonic reads, consistent prefix) and the
concurrency machinery (happens-before, version vectors) needed to reason about it.
*(DDIA Ch 5; assumes data fits one machine — partitioning is Ch 6.)*

## Key ideas

- **[[single-leader-replication]]** — one leader takes writes, streams a replication
  log to followers; sync vs. async (semi-synchronous); follower catch-up;
  **[[failover-and-split-brain|failover]]**.
- **[[replication-log-implementations]]** — statement-based / WAL shipping / logical
  (row-based) / trigger-based; logical logs enable [[change-data-capture]].
- **[[replication-lag]]** → **[[eventual-consistency]]** and three guarantees:
  **[[read-after-write-consistency]]**, **[[monotonic-reads]]**,
  **[[consistent-prefix-reads]]**.
- **[[multi-leader-replication]]** — multi-datacenter, offline clients,
  collaborative editing; the big downside is **[[write-conflict-resolution|write
  conflicts]]**; replication topologies (all-to-all, circular, star).
- **[[leaderless-replication]]** — Dynamo-style; any replica accepts writes;
  **[[quorum-consistency]]** (w + r > n); read repair & anti-entropy;
  **[[sloppy-quorum-and-hinted-handoff]]**.
- **[[happens-before-and-concurrency]]** — defining concurrency; LWW's data loss;
  version numbers → **version vectors** / siblings / tombstones.

## Concepts introduced

- [[single-leader-replication]] · [[multi-leader-replication]] ·
  [[leaderless-replication]] · [[replication-log-implementations]]
- [[failover-and-split-brain]] · [[replication-lag]] · [[eventual-consistency]]
- [[read-after-write-consistency]] · [[monotonic-reads]] ·
  [[consistent-prefix-reads]]
- [[write-conflict-resolution]] · [[quorum-consistency]] ·
  [[sloppy-quorum-and-hinted-handoff]] · [[happens-before-and-concurrency]]
- [[change-data-capture]]

## Comparisons introduced

- [[single-vs-multi-vs-leaderless-replication]]

## Systems / papers referenced

- [[cassandra]], [[riak-bitcask|riak]], [[voldemort]] — Dynamo-style leaderless;
  [[postgresql]]/[[mongodb]] — single-leader; CouchDB — offline multi-leader
- [[dynamo-paper]] (DeCandia 2007), [[lamport-clocks-paper]] (happens-before, 1978)

## Trade-offs & tensions

- **Sync vs. async** — durability/consistency vs. availability/latency (sync follower
  blocks writes if down; async loses unreplicated writes on leader failure).
- **Failover hazards** — lost async writes, split brain, timeout tuning → some teams
  do manual failover.
- **Single vs. multi vs. leaderless** — simplicity/no-conflicts vs. robustness under
  faults/latency at the cost of weak consistency & conflict handling.
- **Quorum w+r>n** — appears to guarantee fresh reads but many edge cases (sloppy
  quorum, concurrent writes, failed writes not rolled back).
- **LWW** — convergence at the cost of silent data loss; safe only with
  immutable/unique keys.

## Connections to other chapters

- Failover/leader election & "split brain", fencing/STONITH → consensus
  ([[ch09-consistency-and-consensus]]); detecting failure via timeout, clock issues →
  [[ch08-the-trouble-with-distributed-systems]].
- The replication log is the Ch 3 append-only log reused; logical-log CDC →
  [[ch11-stream-processing]].
- "Transactions exist so apps can be simpler" → [[ch07-transactions]],
  [[ch09-consistency-and-consensus]]; alternatives → Part III.
- Linearizability vs. quorums, version vectors/causality →
  [[ch09-consistency-and-consensus]]; partitioning counterpart →
  [[ch06-partitioning]].

## Open questions / things to revisit

- How do these consistency models relate to linearizability/causal consistency
  (Ch 9)?
- CRDTs/operational transformation revisited for collaborative apps (Ch 12)?
