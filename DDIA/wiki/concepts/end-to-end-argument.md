---
title: The End-to-End Argument
type: concept
chapters: [12]
tags: [correctness, exactly-once, idempotence, integrity]
status: solid
updated: 2026-05-16
---

# The End-to-End Argument

## Statement

> "The function in question can completely and correctly be implemented only with
> the knowledge and help of the application standing at the endpoints… providing it
> as a feature of the communication system itself is not possible (an incomplete
> version may be useful as a performance enhancement)." — Saltzer, Reed, Clark 1984.
*(DDIA Ch 12)*

A strong-safety data system (even serializable transactions) does **not** guarantee
the application is free from data loss/corruption: app bugs occur, people make
mistakes.

## Duplicate suppression needs end-to-end

Exactly-once (idempotence) must span *every hop*. TCP dedups within one connection;
stream processors dedup at the message level; database transactions are tied to a
connection — but none stop a **user** resubmitting a timed-out POST (browser "submit
again?"; the request retried end-to-end is a *separate* transaction). A non-idempotent
money transfer thus risks double-charging — "real banks don't work like this".

**Fix: an end-to-end operation identifier** (UUID / hash of form fields) passed from
client → all the way to the database, enforced by a uniqueness constraint on a
`requests` table (relational DBs maintain uniqueness even at weak isolation, unlike an
app-level check-then-insert which fails under non-serializable — [[write-skew-and-phantoms]]).
That table doubles as an event log (→ [[event-sourcing]]).

## Generalization

Same argument for **integrity checks** (Ethernet/TCP/TLS checksums catch network
corruption, not sender/receiver software bugs or disk corruption → need end-to-end
checksums) and **encryption** (only end-to-end protects against server compromise).
Low-level reliability is still useful (reduces higher-level fault probability) but
**not sufficient** alone. Transactions are a great abstraction (collapse many faults
to commit/abort) but not enough, and expensive across heterogeneous tech → people
re-implement fault tolerance in app code, usually incorrectly → we need
fault-tolerance abstractions that give application-specific end-to-end correctness
with good performance.

## Related concepts

- [[stream-fault-tolerance]] (exactly-once / idempotence) · [[fencing-tokens]] ·
  [[enforcing-constraints-in-dataflow]] · [[timeliness-vs-integrity]] ·
  [[distributed-transactions-xa]]

## Sources

DDIA Ch 12 ("The End-to-End Argument for Databases").
