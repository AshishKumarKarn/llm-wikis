---
title: Fault-Tolerant Consensus
type: concept
chapters: [9]
tags: [distributed, consensus, paxos, raft, fault-tolerance]
status: solid
updated: 2026-05-16
---

# Fault-Tolerant Consensus

## Definition

Get several nodes to **agree on something** such that the decision is **irrevocable**.
Formal properties: *(DDIA Ch 9)*

- **Uniform agreement** — no two nodes decide differently.
- **Integrity** — no node decides twice.
- **Validity** — a decided value was proposed by some node.
- **Termination** — every non-crashed node eventually decides (the **liveness** /
  fault-tolerance property; the other three are safety).

A single "dictator" node satisfies the first three but not termination (fails if it
crashes — exactly the 2PC coordinator problem).

## FLP and the majority requirement

**FLP** (Fischer-Lynch-Paterson): no *deterministic* consensus algorithm in the
*asynchronous* model (no clocks/timeouts). But with timeouts (imperfect failure
detection) or randomness, consensus **is** solvable in practice. Termination requires
**a majority of nodes working** (a quorum) — but safety (agreement/integrity/
validity) holds even if a majority fails. Assumes **non-Byzantine** (Byzantine
consensus needs >2/3 honest).

## Algorithms ≈ total order broadcast

Best-known: **Viewstamped Replication, Paxos (Multi-Paxos), Raft, Zab**. They decide
a *sequence* of values → they implement **[[total-order-broadcast]]** (= repeated
rounds of consensus). Don't implement your own — it's hard.

## Epoch numbering & overlapping quorums

Single-leader replication *is* total order broadcast — but who picks the leader?
Manual = dictator (no termination); automatic failover needs consensus → **"to elect
a leader we need consensus; to solve consensus we need a leader"**. Break the loop:
protocols use an **epoch number** (ballot/view/term) — unique leader *per epoch*,
higher epoch wins. A leader must collect votes from a **quorum** for each decision;
the quorum for a proposal must **overlap** with the most recent leader-election
quorum, so a leader learns if it's been superseded. (vs. 2PC: coordinator not
elected; consensus needs only a *majority*, not all; has a recovery process.)

## Limitations

Synchronous-replication cost; needs a **strict majority** (≥3 for 1 failure); mostly
**static membership**; timeout-based → geo-distributed deployments suffer frequent
spurious leader elections (Raft edge cases). Single-leader DBs still need consensus
for leadership — having a leader just "kicks the can down the road" (consensus less
often).

## Related concepts

- [[total-order-broadcast]] · [[linearizability]] · [[two-phase-commit]] ·
  [[coordination-services]] · [[truth-by-majority]] · [[failover-and-split-brain]]
- [[flp-impossibility]] · [[paxos-raft-paper]]

## Sources

DDIA Ch 9 ("Fault-Tolerant Consensus").
