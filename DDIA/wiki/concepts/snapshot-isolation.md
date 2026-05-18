---
title: Snapshot Isolation (Repeatable Read)
type: concept
chapters: [7]
tags: [transactions, isolation, mvcc, concurrency-control]
status: solid
updated: 2026-05-16
---

# Snapshot Isolation (Repeatable Read)

## Definition

Each transaction reads from a **consistent snapshot** of the database — all data
committed *as of the transaction's start*; later changes by others are invisible.
Solves **read skew / nonrepeatable read** (e.g. Alice sees $900 because she read one
account before and one after a $100 transfer). *(DDIA Ch 7)*

## Why it matters

A boon for **long-running read-only** work — backups, analytic queries, integrity
checks — which return nonsense if they observe the DB at multiple points in time.
Supported by PostgreSQL, MySQL/InnoDB, Oracle, SQL Server.

## Implementation (MVCC + visibility rules)

Built on [[multi-version-concurrency-control]]: write locks prevent dirty writes, but
**reads take no locks** — *readers never block writers, writers never block readers*.
Each transaction gets a monotonically increasing **txid**; rows carry
`created_by`/`deleted_by`; an update = delete + create. **Visibility rule**: an
object is visible iff its creating transaction had committed before the reader started
**and** it isn't deleted by a transaction that had committed before the reader
started; ignore in-progress, aborted, and later-txid writes. Indexes either point to
all versions (filter on read) or use append-only/copy-on-write [[b-tree]]s
(CouchDB/Datomic/LMDB — each write creates a new immutable root = a snapshot).

## The "repeatable read" naming chaos

The SQL standard (based on System R 1975) predates snapshot isolation and defines a
flawed, ambiguous **repeatable read**. PostgreSQL/MySQL call snapshot isolation
"repeatable read"; Oracle calls it "serializable"; IBM DB2 uses "repeatable read" for
true serializability. **"Nobody really knows what repeatable read means."**

## Limitation

Prevents read skew & dirty reads/writes but **not** [[lost-updates]] or
[[write-skew-and-phantoms]] (write skew needs true [[serializability]]). Matrix:
[[isolation-levels]].

## Related concepts

- [[read-committed]] · [[multi-version-concurrency-control]] · [[lost-updates]] ·
  [[write-skew-and-phantoms]] · [[serializable-snapshot-isolation]]

## Sources

DDIA Ch 7 ("Snapshot Isolation and Repeatable Read").
