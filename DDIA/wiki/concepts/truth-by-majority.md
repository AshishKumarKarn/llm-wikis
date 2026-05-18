---
title: Truth Defined by the Majority (Quorum)
type: concept
chapters: [8]
tags: [distributed, quorum, consensus, leader]
status: solid
updated: 2026-05-16
---

# Truth Defined by the Majority (Quorum)

## The problem

A node cannot know anything for sure — only guess from messages received (or not).
Three nightmares: an **asymmetric link** node declared dead though working; a
semi-disconnected node that can't refute it; a **GC-paused** node that revives
believing no time passed. Moral: **a node cannot trust its own judgment of a
situation**. *(DDIA Ch 8)*

## The resolution: quorum

A distributed system can't rely on a single node (it may fail anytime). Decisions —
**including declaring a node dead** — require a **quorum**: votes from a minimum
number of nodes. Usually an **absolute majority** (>½): tolerates failures (3 nodes →
1; 5 → 2) and is safe because there can be only **one majority** (no two conflicting
majorities). Even a node that "feels alive" must abide by the quorum and step down.
(Detailed → [[ch09-consistency-and-consensus]].)

## The leader and the lock

Systems often need exactly one of something (one partition leader to avoid
[[failover-and-split-brain|split brain]]; one lock holder; one username owner). A
node believing it is "the chosen one" doesn't mean the quorum agrees — it may have
been demoted during a pause/partition. If it keeps acting, it can corrupt data (the
HBase lease bug, see [[process-pauses]]). Guard with **[[fencing-tokens]]**.

## Related concepts

- [[fencing-tokens]] · [[byzantine-faults]] · [[process-pauses]] ·
  [[unreliable-networks]]
- [[quorum-consistency]] (Ch 5 read/write quorums) ·
  [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 8 ("The Truth Is Defined by the Majority").
