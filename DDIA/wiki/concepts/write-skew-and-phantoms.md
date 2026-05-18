---
title: Write Skew & Phantoms
type: concept
chapters: [7]
tags: [transactions, concurrency-control, race-condition, serializability]
status: solid
updated: 2026-05-16
---

# Write Skew & Phantoms

## Definition

**Write skew**: two transactions read the same objects, then update *different*
objects, each invalidating the other's premise. A generalization of [[lost-updates]]
(special case = same object). Canonical example: two on-call doctors each check "≥2
on call?" (both see 2), each goes off call → **zero doctors on call**. *(DDIA Ch 7)*

## More examples (same pattern)

Meeting-room double-booking, multiplayer-game illegal move, claiming a username,
preventing double-spending. Pattern: (1) `SELECT` checks a precondition, (2) app
decides based on it, (3) a write changes that precondition — repeating the `SELECT`
after the write would now give a different result.

## Phantoms

A **phantom** = a write in one transaction changing the result of another's search
query. When the precondition is "no rows match" (booking, username), there's no
existing row to lock (`SELECT FOR UPDATE` can't lock nothing). Snapshot isolation
avoids phantoms in *read-only* queries but not in read-write write-skew cases.

## Prevention (options are restricted)

- Atomic single-object ops don't help (multiple objects).
- Auto lost-update detection doesn't help — write skew is **not** detected by
  PostgreSQL/MySQL repeatable read, Oracle/SQL Server snapshot isolation.
- Multi-object **constraints** (≥1 doctor on call) — most DBs lack them; emulate with
  triggers/materialized views.
- **Explicit `SELECT FOR UPDATE`** locking the rows the decision depends on (works
  when rows exist).
- **Materializing conflicts** — invent lock rows (e.g. all room×timeslot rows) so a
  phantom becomes a concrete lock conflict. Ugly, error-prone — *last resort*.
- **True [[serializability]]** is the proper fix.

## Related concepts

- [[lost-updates]] · [[serializability]] · [[two-phase-locking]] (predicate /
  index-range locks) · [[serializable-snapshot-isolation]] · [[isolation-levels]]
- [[write-conflict-resolution]] — the replicated booking-conflict cousin

## Sources

DDIA Ch 7 ("Write Skew and Phantoms").
