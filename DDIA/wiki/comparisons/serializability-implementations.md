---
title: "Serializability: Serial Execution vs. 2PL vs. SSI"
type: comparison
chapters: [7]
tags: [transactions, serializability, comparison]
status: solid
updated: 2026-05-16
---

# Serializability: Serial Execution vs. 2PL vs. SSI

The three ways to implement [[serializability]]. *(DDIA Ch 7)*

| | [[actual-serial-execution\|Serial execution]] | [[two-phase-locking\|2PL]] | [[serializable-snapshot-isolation\|SSI]] |
|---|---|---|---|
| Approach | no concurrency (1 thread) | pessimistic locking | optimistic (check at commit) |
| Blocking | n/a (serial) | readers ⇄ writers block; deadlocks | **none** — non-blocking |
| Throughput ceiling | one CPU core (partition to scale) | reduced concurrency | scales (FoundationDB across nodes) |
| Latency | predictable if txns tiny | unstable, high p99 under contention | predictable; lock-free reads |
| Weakness | dataset in memory; short txns; cross-partition slow | lock overhead, deadlock retries | aborts under high contention; needs short RW txns |
| Maturity | since ~2007 | ~30 yrs, standard | since 2008, newest |
| Examples | VoltDB, Redis, Datomic | MySQL InnoDB / SQL Server serializable, DB2 RR | PostgreSQL ≥9.1, FoundationDB |

## How to choose

- **Serial execution**: low write throughput, small in-memory transactions, can use
  stored procedures, partitionable without cross-partition coordination.
- **2PL**: general-purpose, correctness over performance; tolerate latency
  instability.
- **SSI**: read-heavy, want predictable latency & scalability, contention not too
  high, transactions short. DDIA: "possibility of becoming the new default".

## Related

- [[serializability]] · [[isolation-levels]] · [[snapshot-isolation]]
- [[ch09-consistency-and-consensus]] — generalizing these across nodes (2PC,
  distributed SSI)

## Sources

DDIA Ch 7 (Summary).
