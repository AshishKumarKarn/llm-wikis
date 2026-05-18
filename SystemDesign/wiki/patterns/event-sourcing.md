---
title: Event Sourcing Pattern
type: pattern
tags: [event-sourcing, audit, event-driven, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Event Sourcing Pattern

Stores state as a **sequence of immutable events** rather than the current snapshot.
Current state is reconstructed by replaying events from the event store
(see [[sources/design-patterns-study-guide]]).

## Traditional CRUD vs Event Sourcing
| CRUD | Event Sourcing |
|---|---|
| `balance = 500` | `AccountOpened, MoneyDeposited(1000), MoneyWithdrawn(500)` |
| No history | Full audit trail |
| "What is the state?" only | "What happened, when, and why?" |
| ❌ Can't replay | ✅ Time travel, replay for debugging |

## How it works
1. **Command** — user performs an action (e.g., PlaceOrder).
2. **Event created** — if valid, an immutable event is recorded: `OrderPlaced`.
3. **Event stored** — appended to the event store (append-only, never updated).
4. **State rebuilt** — replay all events to compute current state.

## Event Store
- Append-only; events are immutable; acts as source of truth.
- Storage options: Kafka, EventStoreDB, relational table with sequence column.

## Read Models (Projections)
Replaying all events on every read is expensive. Instead:
- Build **read-optimized projections** (denormalized views) asynchronously.
- Combined with [[patterns/cqrs]]: event store is the write model; projections
  are the read model.

## Benefits & trade-offs
| Benefit | Trade-off |
|---|---|
| Complete audit trail | Higher complexity |
| Easy debugging & time travel | Event schema versioning needed |
| Natural event-driven fit | Steep learning curve |
| Supports complex business workflows | Not suited for simple CRUD |

## When to use
Auditability is critical · complex business logic with state transitions ·
event replay is valuable (reporting, debugging, projections).

## When to avoid
Simple CRUD systems · teams new to distributed systems.

## Related
[[patterns/cqrs]] (natural pairing) · [[patterns/outbox-pattern]] ·
[[concepts/delivery-semantics]]
