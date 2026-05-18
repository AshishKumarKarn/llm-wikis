---
title: Causal Consistency & Ordering
type: concept
chapters: [9]
tags: [distributed, consistency, causality, ordering]
status: solid
updated: 2026-05-16
---

# Causal Consistency & Ordering

## Definition

Causality imposes an ordering: cause before effect, send before receive, question
before answer. A system that obeys this ordering is **causally consistent**. Snapshot
isolation provides causal consistency (a consistent snapshot is *consistent with
causality* — if it has the answer it has the question). *(DDIA Ch 9)*

## Partial vs. total order (the key distinction)

- **Linearizability** = a **total order**: every pair of operations is comparable;
  one single timeline; *no concurrent operations*.
- **Causality** = a **partial order**: causally-related operations are ordered,
  concurrent ones are *incomparable* (timeline branches & merges — like a Git
  history; cf. [[happens-before-and-concurrency]]).

## Linearizability is stronger than causal consistency

**Linearizability implies causality** (so it auto-preserves cross-channel causality —
why linearizable systems are simple). But it's costly ([[cap-theorem]],
network-delay-bound). **Causal consistency is the strongest model that does NOT slow
down under network delay and stays available under partitions** — CAP doesn't apply.
Many systems that *seem* to need linearizability actually only need causal
consistency; new research databases preserve causality with eventual-consistency-like
performance (not yet mainstream).

## Capturing causal dependencies

A replica must process an operation only after all causally-preceding operations.
Track "knowledge" of each node — generalize **version vectors** across the whole
database (not just one key); pass the read version back on writes (as in Fig 5-13 and
SSI conflict detection). Tracking *all* causal dependencies is impractical → use
**sequence numbers** ([[lamport-timestamps]]) instead.

## Related concepts

- [[linearizability]] · [[lamport-timestamps]] · [[happens-before-and-concurrency]]
  · [[consistent-prefix-reads]] · [[snapshot-isolation]] · [[cap-theorem]]

## Sources

DDIA Ch 9 ("Ordering and Causality").
