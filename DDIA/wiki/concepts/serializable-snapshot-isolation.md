---
title: Serializable Snapshot Isolation (SSI)
type: concept
chapters: [7]
tags: [transactions, serializability, optimistic, ssi]
status: solid
updated: 2026-05-16
---

# Serializable Snapshot Isolation (SSI)

## Definition

A 2008 algorithm (Cahill) giving **full serializability** with only a small penalty
over [[snapshot-isolation]]. **Optimistic**: transactions proceed without blocking;
at commit the DB checks whether isolation was violated and aborts if so. PostgreSQL
serializable (≥9.1); FoundationDB (distributed). *(DDIA Ch 7)*

## Pessimistic vs. optimistic

[[two-phase-locking|2PL]] and serial execution are **pessimistic** (wait/block if
anything *might* go wrong). SSI is **optimistic** — good when there's spare capacity
and contention is low; bad under high contention (many aborts, retry load worsens
things). Commutative atomic ops reduce contention.

## How it works

SSI = snapshot isolation ([[multi-version-concurrency-control|MVCC]] consistent
reads) **+** detection of serialization conflicts. A transaction acts on a
**premise** (a fact true at start, e.g. "2 doctors on call") that may become stale.
Two detections:

- **Stale MVCC read**: track when a transaction *ignored* another's write via
  visibility rules; at commit, if any ignored write has since committed, abort. Wait
  until commit (read-only transactions needn't abort; the writer may yet abort).
- **Writes affecting prior reads**: like [[two-phase-locking|index-range locks]] but
  **non-blocking** — record which transactions read an index range; a later write to
  that range acts as a **tripwire** notifying readers their read may be stale;
  whichever commits after a conflicting committed write aborts.

## Performance

Tracking granularity trades precision vs. bookkeeping overhead. Big win over 2PL:
**no blocking on locks** → predictable latency, lock-free read-only queries. Unlike
serial execution, **not limited to one CPU core** (FoundationDB distributes conflict
detection across machines, serializable across partitions). Abort rate dominates
performance → needs **short read-write** transactions (long read-only OK); less
sensitive to slow transactions than 2PL/serial.

## Related concepts

- [[serializability]] · [[snapshot-isolation]] ·
  [[multi-version-concurrency-control]] · [[two-phase-locking]] ·
  [[actual-serial-execution]] · [[serializability-implementations]]

## Sources

DDIA Ch 7 ("Serializable Snapshot Isolation (SSI)"). Refs: Cahill et al. (SIGMOD
2008); Ports & Grittner (SSI in PostgreSQL, VLDB 2012).
