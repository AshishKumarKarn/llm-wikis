---
title: State, Streams & Immutability (the Log↔State Duality)
type: concept
chapters: [11]
tags: [stream, immutability, cqrs, derived-data]
status: solid
updated: 2026-05-16
---

# State, Streams & Immutability (the Log↔State Duality)

## The core idea

Mutable state and an append-only log of immutable events are **two sides of the same
coin**. Whatever the current state (available seats, account balance), there was
always a sequence of events that produced it. Mathematically: **state = integral of
the event stream over time; change stream = derivative of state**. *(DDIA Ch 11)*

> Pat Helland: *"The truth is the log. The database is a cache of a subset of the
> log"* — the latest value of each record. (Log compaction bridges log ↔ state.)

## Advantages of immutable events

- **Auditability** (centuries-old in accounting: never erase a ledger entry; add a
  compensating one). Powerful even outside regulated systems.
- **Human fault tolerance** (cf. Ch 10): recover from buggy code by replaying, not
  destructive overwrite.
- **Captures more than current state**: "added then removed item from cart" is
  analytically useful, lost in a mutable DB.
- **Multiple read views from one log** (Druid, Pistachio, Kafka Connect sinks): add a
  new feature by building a new read-optimized view alongside the old, then retire
  the old — easier than schema migration. This is **CQRS** (Command Query
  Responsibility Segregation). Denormalization debates become moot — translate the
  write-optimized log into any denormalized read view, kept consistent by the
  translation (the Twitter timeline fan-out is exactly this).

## Concurrency control

Downside: log-derived views are asynchronous → read-your-writes problem
([[read-after-write-consistency]]); fix via synchronous view update (transaction) or
[[total-order-broadcast]]. Upside: a self-contained event = a single atomic append →
much of the need for multi-object transactions disappears; same-partition log +
single-threaded consumer needs no write concurrency control ([[actual-serial-execution]]).

## Limits of immutability

Feasibility depends on **churn**: append-mostly is easy; high update/delete rates →
history grows huge, fragmentation, GC/compaction performance critical. **Deletion**
for privacy/legal (GDPR) needs to *rewrite history* (Datomic **excision**, Fossil
**shunning**) — truly deleting is hard (copies in storage engines, backups); more
"hard to retrieve" than "impossible".

## Related concepts

- [[change-data-capture]] · [[event-sourcing]] ·
  [[systems-of-record-and-derived-data]] · [[materialized-views-and-data-cubes]] ·
  [[normalization-and-denormalization]] · [[load-parameters]] (Twitter timeline)

## Sources

DDIA Ch 11 ("State, Streams, and Immutability").
