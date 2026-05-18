---
title: Distributed Transactions in Practice (XA)
type: concept
chapters: [9]
tags: [distributed, transactions, xa, exactly-once]
status: developing
updated: 2026-05-16
---

# Distributed Transactions in Practice (XA)

## Two types

- **Database-internal**: all participants run the same DB software (VoltDB, MySQL
  Cluster NDB) — can optimize freely, often work well (a distributed SSI is
  possible).
- **Heterogeneous**: different technologies (two vendors' DBs, or a DB + message
  broker) — must share one atomic commit protocol; **much harder**. *(DDIA Ch 9)*

## XA (eXtended Architecture)

A 1991 standard for 2PC across heterogeneous systems — **a C API, not a network
protocol** (Java: JTA over JDBC/JMS). Coordinator usually a **library in the app
process**, logging decisions to local disk. Enables **exactly-once message
processing**: atomically commit a message-broker ack + the DB writes → safe redelivery
on failure (only if *all* side effects participate; a non-XA email server can't, so
emails may double-send).

## Why it hurts (limitations)

- The **coordinator is itself a database** of transaction outcomes — yet often not
  replicated/HA → a **single point of failure**; makes "stateless" app servers
  stateful (its log is crucial durable state).
- **Holding locks while in doubt**: a stuck-in-doubt transaction holds row
  (exclusive, and shared if 2PL) locks until resolved — coordinator down 20 min ⇒
  locks held 20 min; log lost ⇒ forever, blocking other transactions → large parts
  of the app unavailable.
- **Orphaned in-doubt transactions** need *manual* admin resolution under outage
  stress; **heuristic decisions** = "probably breaking atomicity" escape hatch.
- XA is a **lowest common denominator**: can't detect cross-system deadlocks, doesn't
  work with SSI; 2PC needs *all* participants up → **amplifies failures** (counter to
  fault tolerance). MySQL distributed transactions reported >10× slower.

> Alternatives that achieve similar ends without this pain → Part III
> ([[ch11-stream-processing]], [[ch12-the-future-of-data-systems]]).

## Related concepts

- [[two-phase-commit]] · [[consensus]] · [[two-phase-locking]] ·
  [[transaction-aborts-and-retries]] · [[ch11-stream-processing]] (exactly-once)

## Sources

DDIA Ch 9 ("Distributed Transactions in Practice").
