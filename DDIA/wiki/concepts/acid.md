---
title: ACID (Atomicity, Consistency, Isolation, Durability)
type: concept
chapters: [7]
tags: [transactions, acid, durability]
status: solid
updated: 2026-05-16
---

# ACID (Atomicity, Consistency, Isolation, Durability)

## Definition

The safety guarantees of transactions, coined 1983 (Härder & Reuter). In practice
implementations differ wildly (esp. **isolation**) — "ACID compliant" is now largely
a **marketing term**. BASE ("Basically Available, Soft state, Eventual consistency")
is even vaguer — effectively just "not ACID". *(DDIA Ch 7)*

## The four letters

- **Atomicity** — *not* about concurrency (that's I). If a fault occurs partway
  through a transaction's writes, the transaction **aborts** and all its writes are
  discarded → safe to retry without partial-failure ambiguity. Better named
  *abortability*.
- **Consistency** — an **application-defined** notion of valid invariants (e.g.
  credits = debits). The app must write transactions that preserve invariants; the DB
  only enforces *some* (FK/uniqueness constraints). So **C is a property of the
  application, not the database** — "tossed in to make the acronym work" (Hellerstein
  via Härder & Reuter). Note: this is a 4th, distinct meaning of "consistency"
  (vs. replica consistency, consistent hashing, CAP/linearizability).
- **Isolation** — concurrently executing transactions don't step on each other;
  classically formalized as **serializability** (each transaction as if alone). In
  practice serializable is rare (cost) → weak [[isolation-levels]].
- **Durability** — once committed, data survives crashes (disk + WAL, or
  replication). **No perfect durability**: disk/SSD firmware bugs, fsync violations,
  silent corruption, correlated faults, lost async-replicated writes — use disk +
  replication + backups together; treat "guarantees" with salt.

## Why it matters

ACID exists to **simplify the application**: a large class of errors collapses to
"abort and retry". Atomicity, isolation, durability are DB properties; consistency is
the app's. Not every app needs transactions; weakening/abandoning them can buy
performance/availability — a trade-off, not dogma.

## Related concepts

- [[single-vs-multi-object-transactions]] · [[transaction-aborts-and-retries]]
- [[isolation-levels]] · [[serializability]]
- [[ch09-consistency-and-consensus]] — the CAP/linearizability sense of "consistency"

## Sources

DDIA Ch 7 ("The Meaning of ACID").
