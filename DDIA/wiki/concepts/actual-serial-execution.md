---
title: Actual Serial Execution
type: concept
chapters: [7]
tags: [transactions, serializability, stored-procedures]
status: developing
updated: 2026-05-16
---

# Actual Serial Execution

## Definition

Remove concurrency entirely: execute **one transaction at a time, serially, on a
single thread**. Serializable by definition. Adopted ~2007 (VoltDB/H-Store, Redis,
Datomic). *(DDIA Ch 7)*

## What made it feasible

- **RAM cheap enough** to keep the active dataset in memory ([[in-memory-databases]])
  — no disk waits.
- OLTP transactions are **short, few reads/writes**; long analytic reads run
  separately on a snapshot ([[snapshot-isolation]]).

## Stored procedures

Interactive one-statement-at-a-time transactions would waste the single thread
waiting on network round-trips, so the whole transaction is submitted ahead of time
as a **stored procedure**. Old reputation bad (vendor-specific PL/SQL, hard to
debug/version/monitor, perf-sensitive) — modern systems use general languages (VoltDB
Java/Groovy, Datomic Java/Clojure, Redis Lua). VoltDB also replicates by running the
*same deterministic stored procedure* on each replica.

## Scaling via partitioning

Throughput limited to one CPU core. **Partition** so each transaction touches one
partition → one thread per partition, linear scaling. Cross-partition transactions
need lock-step coordination — ~1000/s in VoltDB, orders of magnitude slower, can't
scale by adding machines. Multiple secondary indexes tend to force cross-partition
coordination ([[local-vs-global-secondary-indexes]]).

## Constraints (summary)

Every transaction small & fast (one slow one stalls all); active dataset in memory;
write throughput fits one core or partitions without cross-partition coordination.

## Related concepts

- [[serializability]] · [[two-phase-locking]] · [[serializable-snapshot-isolation]]
  · [[serializability-implementations]]
- [[in-memory-databases]] · [[ch06-partitioning]]

## Sources

DDIA Ch 7 ("Actual Serial Execution").
