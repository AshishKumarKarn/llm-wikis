---
title: Happens-Before & Concurrency (Version Vectors)
type: concept
chapters: [5]
tags: [concurrency, causality, version-vectors, lww]
status: solid
updated: 2026-05-16
---

# Happens-Before & Concurrency (Version Vectors)

## Definition

Operation **A happens before B** if B knows about / depends on / builds upon A. Two
operations are **concurrent** if *neither* happens before the other (neither knows
about the other). For any A, B: A→B, B→A, or concurrent. *(DDIA Ch 5)*

> Concurrency isn't about literal time overlap (clocks are unreliable — Ch 8). Two
> ops are concurrent if both are *unaware of each other*, even if separated in
> physical time (slow/interrupted network). (Analogy to relativity / light cones.)

## Last write wins (LWW) and its danger

Force an arbitrary order via timestamps, keep the biggest, discard the rest. Achieves
convergence but **at the cost of durability**: concurrent writes all acked to clients
are silently dropped except one; LWW can even drop *non*-concurrent writes under
clock skew. Cassandra's only conflict method; optional in Riak. **Safe only if each
key is written once and treated immutable** (e.g. Cassandra UUID keys).

## Capturing happens-before (single replica)

Server keeps a **version number** per key, increments on write, stores it with the
value. A read returns all non-overwritten values + the latest version. A client must
read before writing, and on write send the prior version + the **merged** values. The
server overwrites everything ≤ that version but keeps higher versions as **siblings**
(concurrent values). Merging siblings = the same problem as
[[write-conflict-resolution]] (cart union; deletes need a **tombstone** so removed
items don't reappear; CRDTs automate this).

## Version vectors (multiple replicas)

One version number **per replica per key**; each replica tracks the versions it has
seen from every other. The collection is a **version vector** (Riak 2.0 uses *dotted
version vectors*, sent to clients as "causal context"). It makes it safe to read from
one replica and write back to another (may create siblings, but no data lost if
merged correctly). A version vector ≠ a vector clock (subtle; version vectors are the
right structure for comparing replica state).

## Related concepts

- [[write-conflict-resolution]] · [[leaderless-replication]] ·
  [[quorum-consistency]]
- [[consistent-prefix-reads]] · [[ch08-the-trouble-with-distributed-systems]]
  (clocks) · [[ch09-consistency-and-consensus]] (causality)

## Sources

DDIA Ch 5 ("Detecting Concurrent Writes"). Ref: [[lamport-clocks-paper]] (Lamport,
"Time, Clocks…", 1978).
