---
title: Eventual Consistency
type: concept
chapters: [5]
tags: [consistency, replication, distributed]
status: solid
updated: 2026-05-16
---

# Eventual Consistency

## Definition

If you stop writing and wait long enough, all replicas eventually converge to the
same value. The temporary inconsistency from [[replication-lag]] is resolved over
time. "Eventually" is **deliberately vague** — there is in general *no upper bound*
on how far a replica may lag. *(DDIA Ch 5)*

## Why it matters

Term coined by Terry et al., popularized by Werner Vogels; the "battle cry" of NoSQL
— but **not NoSQL-specific**: async followers in a relational database are equally
eventually consistent. It is the weak baseline that the stronger guarantees
([[read-after-write-consistency]], [[monotonic-reads]],
[[consistent-prefix-reads]], and ultimately linearizability in
[[ch09-consistency-and-consensus]]) strengthen.

## The operability point

For operability you must be able to **quantify "eventual"** — monitor staleness.
Easy with [[single-leader-replication]] (each follower has a log position; lag =
leader position − follower position). Hard with [[leaderless-replication]] (no fixed
write order; with read-repair only, a rarely-read value can be arbitrarily old).
Research (PBS) predicts stale-read probability from n, w, r.

## Related concepts

- [[replication-lag]] · [[read-after-write-consistency]] · [[monotonic-reads]] ·
  [[consistent-prefix-reads]]
- [[quorum-consistency]] · [[ch09-consistency-and-consensus]] (linearizability)
- [[fault-tolerance]] (Ch 1 forward-referenced this)

## Sources

DDIA Ch 5 ("Problems with Replication Lag", "Monitoring staleness"). Refs: Vogels,
"Eventually Consistent" (2008); Bailis et al., PBS (2014).
