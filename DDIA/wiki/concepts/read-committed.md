---
title: Read Committed Isolation
type: concept
chapters: [7]
tags: [transactions, isolation, concurrency-control]
status: solid
updated: 2026-05-16
---

# Read Committed Isolation

## Definition

The most basic useful isolation level. Two guarantees: **no dirty reads** (you only
see committed data) and **no dirty writes** (you only overwrite committed data).
Default in Oracle 11g, PostgreSQL, SQL Server 2012, MemSQL, etc. (Even weaker: *read
uncommitted* prevents dirty writes only.) *(DDIA Ch 7)*

## No dirty reads

A **dirty read** = seeing another transaction's uncommitted writes. Prevented: a
transaction's writes become visible to others only on commit (all at once). Matters
because partial-update views confuse users / cause bad decisions, and a read of
later-rolled-back data is "mind-bending".

## No dirty writes

A **dirty write** = overwriting another transaction's uncommitted write. Prevented by
**row-level write locks** held until commit/abort. Stops mixed-up multi-object writes
(Alice/Bob buying a car: listing to Bob, invoice to Alice). Does **not** prevent the
[[lost-updates|lost-update]] counter race (the second write is *after* the first
commits — not dirty).

## Implementation

Dirty writes: per-object write lock held to end of transaction. Dirty reads: *not*
read locks (a long writer would stall all readers — bad operability); instead the DB
remembers **old committed + new value**, serving the old value to readers until
commit (a 2-version special case of [[multi-version-concurrency-control|MVCC]]).

## Limitation

Still allows **read skew** (nonrepeatable reads) and other anomalies →
[[snapshot-isolation]]. Full matrix: [[isolation-levels]].

## Related concepts

- [[isolation-levels]] · [[snapshot-isolation]] · [[lost-updates]] ·
  [[multi-version-concurrency-control]]

## Sources

DDIA Ch 7 ("Read Committed").
