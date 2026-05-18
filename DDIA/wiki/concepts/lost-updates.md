---
title: Lost Updates (and Preventing Them)
type: concept
chapters: [7]
tags: [transactions, concurrency-control, race-condition]
status: solid
updated: 2026-05-16
---

# Lost Updates (and Preventing Them)

## Definition

A write-write conflict where two transactions do a **read-modify-write cycle**
concurrently; the second write doesn't include the first's modification, so the first
is lost ("clobbered"). E.g. two counter increments 42→43 instead of 44; JSON
list-append; two users overwriting a whole wiki page. *(DDIA Ch 7)*

## Prevention techniques

- **Atomic write operations** — `UPDATE … SET value = value + 1` (concurrency-safe;
  MongoDB doc ops; Redis structures). Usually best when expressible. Implemented via
  exclusive lock on read (cursor stability) or single-threaded execution. ORMs make
  it easy to *accidentally* write unsafe read-modify-write instead.
- **Explicit locking** — `SELECT … FOR UPDATE` to lock rows the app will update
  (needed when logic can't be a DB query, e.g. game-move validation). Easy to forget
  a lock → race.
- **Automatic lost-update detection** — let them run in parallel; the transaction
  manager detects a lost update and aborts+retries. PostgreSQL repeatable read,
  Oracle serializable, SQL Server snapshot isolation do this; **MySQL/InnoDB
  repeatable read does not** (so arguably not true snapshot isolation). Best because
  it needs no special app code.
- **Compare-and-set** — `UPDATE … SET … WHERE id=… AND content='old'`; update only if
  unchanged. Unsafe if the `WHERE` may read an old snapshot — verify your DB.
- **Replicated data**: locks/CAS assume one up-to-date copy → don't apply to
  multi-leader/leaderless. Use conflict siblings + merge
  ([[happens-before-and-concurrency]]); **commutative** atomic ops (counter
  increment, set add) work well (Riak 2.0 CRDT datatypes). **LWW is prone to lost
  updates** and is a common default.

## Related concepts

- [[write-skew-and-phantoms]] (the generalization) · [[snapshot-isolation]] ·
  [[serializability]]
- [[happens-before-and-concurrency]] · [[write-conflict-resolution]] ·
  [[isolation-levels]]

## Sources

DDIA Ch 7 ("Preventing Lost Updates").
