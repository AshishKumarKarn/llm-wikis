---
title: "Ch 7 — Transactions"
type: chapter
chapters: [7]
tags: [transactions, acid, isolation, concurrency-control, serializability]
status: solid
updated: 2026-05-16
---

# Ch 7 — Transactions

## One-paragraph thesis

A **transaction** groups several reads/writes into one logical unit that either fully
**commits** or **aborts** (safe to retry). It is not a law of nature — it exists to
**simplify the application programming model** by letting the DB absorb fault and
concurrency scenarios (its *safety guarantees*). The chapter dismantles hype on both
sides ("transactions don't scale" vs. "ACID is mandatory"), defines **ACID**
precisely, then goes deep on **concurrency control**: the catalog of race conditions
(dirty reads/writes, read skew, lost updates, write skew, phantoms), the **weak
isolation levels** that prevent *some* of them (read committed, snapshot isolation),
and the three ways to get true **serializability** (serial execution, 2PL, SSI).
*(DDIA Ch 7; single-node focus — distributed transactions are Ch 9.)*

## Key ideas

- **[[acid]]** — Atomicity (abortability), Consistency (an *application* property —
  "C tossed in to make the acronym"), Isolation, Durability (no perfect durability).
  Contrast BASE ("not ACID").
- **[[single-vs-multi-object-transactions]]** — why multi-object transactions matter
  (foreign keys, denormalized data, secondary indexes); single-object atomic ops /
  compare-and-set are *not* transactions.
- **[[transaction-aborts-and-retries]]** — the point of aborts is safe retry; the
  caveats (dedup, backoff, side effects, permanent errors).
- **[[isolation-levels]]** (the master reference table) → **[[read-committed]]**,
  **[[snapshot-isolation]]** (MVCC, "repeatable read" naming chaos),
  **[[lost-updates]]**, **[[write-skew-and-phantoms]]**.
- **[[serializability]]** — the only level that prevents *all* race conditions; three
  implementations: **[[actual-serial-execution]]**, **[[two-phase-locking]]**,
  **[[serializable-snapshot-isolation]]** (compared in
  [[serializability-implementations]]).
- **[[multi-version-concurrency-control]]** — the mechanism behind snapshot isolation
  and SSI.

## Concepts introduced

- [[acid]] · [[single-vs-multi-object-transactions]] ·
  [[transaction-aborts-and-retries]]
- [[read-committed]] · [[snapshot-isolation]] ·
  [[multi-version-concurrency-control]] · [[lost-updates]] ·
  [[write-skew-and-phantoms]]
- [[serializability]] · [[actual-serial-execution]] · [[two-phase-locking]] ·
  [[serializable-snapshot-isolation]]

## Comparisons introduced

- [[isolation-levels]] (anomaly × level matrix) · [[serializability-implementations]]

## Systems / papers referenced

- [[postgresql]] (MVCC, SSI since 9.1), VoltDB/H-Store ([[actual-serial-execution]]),
  [[redis]]/Datomic (serial), FoundationDB (distributed SSI), Oracle/MySQL/SQL Server
  (isolation-level naming), [[riak-bitcask|Riak]] 2.0 (commutative CRDT datatypes)
- [[critique-of-ansi-sql-isolation]] (Berenson et al. 1995), SSI (Cahill et al. 2008)

## Trade-offs & tensions

- Strong isolation simplifies apps but costs performance; weak levels are fast but
  leak subtle bugs (real money lost, audits, corruption).
- Serial execution: simple, no lock overhead, but one-CPU-core throughput; needs
  small in-memory transactions, stored procedures, single-partition.
- 2PL: prevents everything, but readers block writers & vice versa, deadlocks,
  unstable tail latency.
- SSI: optimistic — no blocking, scalable, but aborts under high contention; needs
  short read-write transactions.

## Connections to other chapters

- "Transactions exist so apps can be simpler" continues [[replication-lag]]'s thesis.
- MVCC reuses copy-on-write [[b-tree]]s ([[ch03-storage-and-retrieval]]); range locks
  build on B-tree indexes.
- Replicated lost updates / LWW → [[happens-before-and-concurrency]],
  [[write-conflict-resolution]] ([[ch05-replication]]); the booking/write-skew
  example connects to multi-leader conflicts.
- ACID "Consistency" ≠ linearizability (CAP) → [[ch09-consistency-and-consensus]];
  distributed transactions / 2PC, serializability across nodes → Ch 9; partitioning
  cross-partition coordination → [[ch06-partitioning]].

## Open questions / things to revisit

- How do serial execution / 2PL / SSI generalize across nodes (2PC, distributed SSI)?
- Where does ACID "Consistency" land vs. linearizability and causal consistency?
