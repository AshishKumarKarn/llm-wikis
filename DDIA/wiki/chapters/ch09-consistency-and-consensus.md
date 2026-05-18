---
title: "Ch 9 — Consistency and Consensus"
type: chapter
chapters: [9]
tags: [distributed, consistency, consensus, linearizability, ordering]
status: solid
updated: 2026-05-16
---

# Ch 9 — Consistency and Consensus

## One-paragraph thesis

The solutions chapter: general-purpose abstractions with useful guarantees, built
once so applications can ignore the Ch 8 troubles. The most important is
**consensus** — getting all nodes to agree, irrevocably — which is "surprisingly
tricky". The chapter builds toward it through three deeply linked topics:
**linearizability** (the strong single-copy/recency illusion, its cost, CAP);
**ordering & causality** (causal consistency, Lamport timestamps, total order
broadcast); and **distributed transactions & consensus** (2PC/atomic commit,
fault-tolerant consensus, ZooKeeper). The profound insight: linearizable
compare-and-set, atomic commit, total order broadcast, locks, uniqueness, and
membership are **all equivalent to consensus**. *(DDIA Ch 9; ends Part II.)*

## Key ideas

- **[[linearizability]]** — make replicas appear as one copy; a **recency
  guarantee** (total order). Useful for locking/leader-election, uniqueness,
  cross-channel races. Single-leader & consensus *can* be linearizable; multi-
  leader/leaderless usually not (even strict quorums aren't —
  [[quorum-consistency]]). Slow (Attiya-Welch).
- **[[cap-theorem]]** — "Consistent or Available when Partitioned"; narrow, mostly
  historical; the "pick 2 of 3" framing is unhelpful.
- **[[causal-consistency]]** — causality is a **partial order**; linearizability is
  stronger but causal consistency is the strongest model that *doesn't* slow down
  under network delay.
- **[[lamport-timestamps]]** — (counter, node ID) total order consistent with
  causality; but timestamp ordering alone can't implement uniqueness (you don't know
  when the order is final).
- **[[total-order-broadcast]]** — reliable + totally-ordered delivery; ≡ consensus;
  ≡ state-machine replication; build linearizable CAS from it and vice versa.
- **[[two-phase-commit]]** — atomic commit across nodes; the in-doubt /
  coordinator-failure problem (blocking); **not** 2PL.
- **[[distributed-transactions-xa]]** — DB-internal vs. heterogeneous (XA);
  exactly-once; holding locks in doubt; limitations.
- **[[consensus]]** — agreement/integrity/validity/termination; FLP; epoch numbering
  + overlapping quorums; Paxos/Raft/Zab/VSR; single-leader still needs consensus for
  leadership.
- **[[coordination-services]]** — ZooKeeper/etcd/Chubby: linearizable CAS, fencing
  tokens, failure detection, change notifications, membership, service discovery.

## Concepts introduced

- [[linearizability]] · [[cap-theorem]] · [[causal-consistency]] ·
  [[lamport-timestamps]] · [[total-order-broadcast]]
- [[two-phase-commit]] · [[distributed-transactions-xa]] · [[consensus]] ·
  [[coordination-services]]
- [[systems-of-record-and-derived-data]] *(Part III intro)*

## Comparisons introduced

- [[linearizability-vs-serializability]]

## Systems / papers referenced

- [[zookeeper]] (Zab; linearizable CAS, ephemeral nodes, fencing; modeled on
  [[chubby-paper|Chubby]]), etcd (Raft), [[google-spanner]] (consensus),
  VoltDB/Calvin (TOB serializable transactions)
- [[flp-impossibility]] (Fischer-Lynch-Paterson 1985), [[paxos-raft-paper]],
  [[lamport-clocks-paper]] (Lamport timestamps)

## Trade-offs & tensions

- Strong guarantees (linearizability, consensus) cost performance/availability;
  weaker (causal, eventual) are faster and partition-tolerant.
- 2PC: real safety but blocking, coordinator = SPOF/stateful, amplifies failures,
  lowest-common-denominator (no cross-system deadlock/SSI), heuristic decisions
  break atomicity.
- Consensus needs a strict majority (≥3 nodes for 1 failure), static membership,
  timeout-tuned (geo-distributed → frequent elections → poor progress).
- Not everything needs consensus — leaderless/multi-leader cope without it (accept
  branching/merging histories).

## Connections to other chapters

- Builds on [[replication-lag]]/[[eventual-consistency]] (Ch 5),
  [[serializability]] (Ch 7), [[system-models]]/[[truth-by-majority]]/
  [[fencing-tokens]] (Ch 8), [[failover-and-split-brain]] (split brain → consensus).
- ACID "C" ≠ linearizability (Ch 7 noted). Spanner TrueTime (Ch 8) ↔ linearizable
  snapshots. Single-leader replication ↔ total order broadcast.
- Alternatives to distributed transactions / state-machine replication / log →
  Part III ([[ch11-stream-processing]], [[ch12-the-future-of-data-systems]]);
  [[systems-of-record-and-derived-data]] frames Part III.

## Open questions / things to revisit

- How do Part III log-based approaches avoid heterogeneous distributed transactions?
- Where does causal consistency reach production (Ch 12)?
