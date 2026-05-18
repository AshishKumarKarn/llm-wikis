---
title: "Isolation Levels vs. Race Conditions (Master Matrix)"
type: comparison
chapters: [7]
tags: [transactions, isolation, concurrency-control, reference]
status: solid
updated: 2026-05-16
---

# Isolation Levels vs. Race Conditions (Master Matrix)

The reference table for [[ch07-transactions]]: which isolation level prevents which
race condition. *(DDIA Ch 7 Summary)*

## The anomalies

- **Dirty read** — read another transaction's uncommitted write.
- **Dirty write** — overwrite another transaction's uncommitted write.
- **Read skew** (nonrepeatable read) — see different parts of the DB at different
  times.
- **Lost update** — two read-modify-write cycles; one clobbers the other.
- **Write skew** — decide on a premise that a concurrent write invalidates.
- **Phantom** — a write changes the result of another transaction's search query.

## The matrix

| Anomaly | Read Committed | Snapshot Isolation (Repeatable Read) | Serializable |
|---|---|---|---|
| Dirty read | **prevented** | prevented | prevented |
| Dirty write | **prevented** | prevented | prevented |
| Read skew | allowed | **prevented** (MVCC snapshot) | prevented |
| Lost update | allowed | *some impls* auto-detect (not MySQL/InnoDB) | prevented |
| Write skew | allowed | allowed | **prevented** (only here) |
| Phantom | allowed | read-only only | **prevented** (predicate/index-range) |

(Even weaker: *read uncommitted* prevents dirty writes only.)

## Takeaways

- Weak levels protect against *some* anomalies; the app developer must handle the
  rest manually (atomic ops, `SELECT FOR UPDATE`). Real incidents: lost money,
  audits, corruption — "use an ACID DB" misses the point since ACID DBs use weak
  isolation by default.
- Only **[[serializability|serializable]]** prevents *everything* — via
  [[actual-serial-execution]], [[two-phase-locking]], or
  [[serializable-snapshot-isolation]] ([[serializability-implementations]]).
- Naming is chaos: snapshot isolation = "repeatable read" (PostgreSQL/MySQL) or
  "serializable" (Oracle); DB2 "repeatable read" = serializable.

## Related

- [[read-committed]] · [[snapshot-isolation]] · [[lost-updates]] ·
  [[write-skew-and-phantoms]] · [[serializability]]

## Sources

DDIA Ch 7 (Summary; "A Critique of ANSI SQL Isolation Levels", Berenson et al. 1995 →
[[critique-of-ansi-sql-isolation]]).
