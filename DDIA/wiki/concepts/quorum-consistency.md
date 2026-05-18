---
title: Quorum Consistency (w + r > n)
type: concept
chapters: [5]
tags: [replication, quorum, consistency, dynamo]
status: solid
updated: 2026-05-16
---

# Quorum Consistency (w + r > n)

## Definition

With **n** replicas, a write needs **w** acks and a read queries **r** nodes. If
**w + r > n**, the read and write node sets overlap in ≥1 node, so a read sees ≥1
up-to-date copy — a **quorum read/write**. *(DDIA Ch 5)*

## How it tolerates failure

Reads/writes go to all n in parallel; w/r is how many acks you *wait* for.

- w < n ⇒ writes survive a node being down; r < n ⇒ reads do too.
- Common: n odd (3/5), w = r = (n+1)/2. n=3,w=2,r=2 tolerates 1 down; n=5,w=3,r=3
  tolerates 2.
- Quorums needn't be majorities — only the overlap matters (flexible Paxos).
- w + r ≤ n: lower latency / higher availability, but more likely stale reads.

## Limitations (w+r>n is *not* an absolute guarantee)

- **Sloppy quorum** ([[sloppy-quorum-and-hinted-handoff]]) — w and r sets may not
  overlap.
- **Concurrent writes** — no clear "first"; must merge or risk LWW data loss
  ([[happens-before-and-concurrency]]).
- Write concurrent with read — undetermined which value is returned.
- A write that succeeded on < w replicas is **not rolled back** on those where it
  succeeded → later reads may or may not see it.
- A node with the new value fails and is restored from an old replica → count of
  fresh replicas drops below w.
- Timing edge cases (linearizability & quorums → [[ch09-consistency-and-consensus]]).

> Dynamo-style stores are optimized for eventual consistency; w/r tune the
> *probability* of staleness, not absolute guarantees. You usually do **not** get
> read-after-write / monotonic / consistent-prefix — stronger guarantees need
> transactions or consensus ([[ch07-transactions]], [[ch09-consistency-and-consensus]]).

## Related concepts

- [[leaderless-replication]] · [[sloppy-quorum-and-hinted-handoff]] ·
  [[eventual-consistency]] · [[happens-before-and-concurrency]]

## Sources

DDIA Ch 5 ("Quorums for reading and writing", "Limitations of Quorum Consistency").
