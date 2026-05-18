---
title: Transaction Aborts & Retries
type: concept
chapters: [7]
tags: [transactions, error-handling, atomicity]
status: developing
updated: 2026-05-16
---

# Transaction Aborts & Retries

## The principle

A defining feature of ACID transactions: on error, **abort** rather than leave things
half-finished — so the application can **safely retry**. The whole point of aborts is
to enable safe retries. *(DDIA Ch 7)*

## Reality check

Many ORMs (Rails ActiveRecord, Django) **don't** retry — the error bubbles up, user
input is lost. A shame, since retry is the intended mechanism. And retry isn't
perfect:

- If it actually committed but the **ack was lost**, retry performs it **twice** —
  need app-level **deduplication/idempotence**.
- If the error was **overload**, retry worsens it — limit retries, **exponential
  backoff**, treat overload errors differently.
- Only retry **transient** errors (deadlock, isolation violation, transient network,
  failover); a **permanent** error (constraint violation) won't succeed on retry.
- **Side effects outside the DB** (sending an email) may fire even if the transaction
  aborts → use 2PC to commit several systems together ([[ch09-consistency-and-consensus]]).
- If the client crashes mid-retry, the data it was writing is lost.

## Related concepts

- [[acid]] · [[single-vs-multi-object-transactions]]
- [[ch09-consistency-and-consensus]] — two-phase commit; idempotence →
  [[ch11-stream-processing]]

## Sources

DDIA Ch 7 ("Handling errors and aborts").
