---
title: Two-Phase Locking (2PL)
type: concept
chapters: [7]
tags: [transactions, serializability, locking, concurrency-control]
status: solid
updated: 2026-05-16
---

# Two-Phase Locking (2PL)

## Definition

For ~30 years the only widely-used serializability algorithm (a.k.a. SS2PL).
**Pessimistic**. Distinct from 2PC (two-phase commit, Ch 9). Shared/exclusive locks
per object: readers block writers **and writers block readers** (unlike snapshot
isolation's "readers never block writers"). *(DDIA Ch 7)*

## How it works

- Read → acquire **shared** lock (many readers OK); write → **exclusive** lock (sole
  holder); read-then-write → upgrade shared→exclusive.
- "**Two-phase**": phase 1 acquire locks (while executing), phase 2 release them all
  (at commit/abort). Held to end of transaction.
- Provides full serializability → prevents lost updates & write skew. Used by MySQL
  InnoDB / SQL Server serializable, DB2 repeatable read.
- **Deadlocks** are frequent; the DB detects and aborts one (app retries).

## Predicate & index-range locks (for phantoms)

To prevent [[write-skew-and-phantoms|phantoms]], a **predicate lock** locks *all
objects matching a search condition* — including ones that don't exist yet. Predicate
locks perform poorly, so most DBs use **index-range (next-key) locking**: a safe
*over*-approximation attached to an index entry (e.g. lock all of room 123, or all
rooms in the time window); fall back to a whole-table lock if no suitable index.

## Performance (why it's not universal)

Lock overhead + drastically **reduced concurrency**: any potential race makes one
transaction wait, with no time limit (traditional DBs allow long transactions).
Unstable, very high tail latencies under contention ([[response-time-percentiles]]);
one slow/large transaction can stall the system. Frequent deadlock-abort retries
waste work.

## Related concepts

- [[serializability]] · [[actual-serial-execution]] ·
  [[serializable-snapshot-isolation]] · [[serializability-implementations]]
- [[write-skew-and-phantoms]] · [[b-tree]] (where range locks attach) ·
  [[ch09-consistency-and-consensus]] (2PC — *not* this)

## Sources

DDIA Ch 7 ("Two-Phase Locking (2PL)").
