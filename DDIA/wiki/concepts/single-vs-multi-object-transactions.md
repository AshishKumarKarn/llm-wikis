---
title: Single-Object vs. Multi-Object Transactions
type: concept
chapters: [7]
tags: [transactions, atomicity, isolation]
status: developing
updated: 2026-05-16
---

# Single-Object vs. Multi-Object Transactions

## Definition

[[acid|Atomicity & isolation]] usually concern modifying **several objects** (rows/
docs) at once (a multi-object transaction). They also apply to a **single object**.
*(DDIA Ch 7)*

## Single-object writes

Storage engines almost universally provide atomicity (via a crash-recovery log) and
isolation (via a per-object lock) for one object — so a half-sent 20 KB JSON, a
power-failure splice, or a partial read don't happen. Some offer atomic
**increment** and **compare-and-set** (write only if value unchanged). These prevent
[[lost-updates]] but are **not transactions** — calling CAS "lightweight
transaction"/"ACID" is misleading marketing; a transaction groups *multiple
operations on multiple objects*.

## Why multi-object transactions matter

- **Foreign keys / graph edges** must stay valid across rows.
- **Denormalized data** (document model lacking joins encourages it) must be updated
  together or it goes out of sync (the email unread-counter dirty-read example).
- **Secondary indexes** are separate objects — without isolation a record can appear
  in one index but not another.

Possible without transactions, but error handling gets much harder and concurrency
bugs creep in. Many distributed stores dropped multi-object transactions (hard across
partitions, availability/perf cost) — nothing fundamentally prevents them
(distributed transactions → [[ch09-consistency-and-consensus]]). Leaderless stores
work "best effort" (won't undo) — error recovery is the app's job.

## Related concepts

- [[acid]] · [[lost-updates]] · [[transaction-aborts-and-retries]]
- [[normalization-and-denormalization]] · [[secondary-indexes]] ·
  [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 7 ("Single-Object and Multi-Object Operations").
