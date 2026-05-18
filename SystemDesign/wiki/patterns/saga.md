---
title: Saga Pattern
type: pattern
tags: [distributed-transactions, microservices, patterns, consistency]
sources: [system-design, design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Saga Pattern

Manages long-running distributed transactions by breaking them into a sequence of local
transactions, each with a **compensating transaction** for rollback. Replaces 2PC
(see [[sources/design-patterns-study-guide]], [[sources/system-design-study-guide]]).

## Why not 2PC?
2PC requires a centralized coordinator holding distributed locks — single point of
failure, blocking under partitions, poor availability at scale. Almost always rejected in
modern microservices.

## Approaches
- **Choreography (event-driven):** each service emits an event after its step; the next
  listens and reacts. Fully decoupled; hard to trace; no central oversight of the overall
  transaction.
- **Orchestration (SEC — Saga Execution Component):** central component directs each
  step, consults the Saga log, and invokes compensating transactions on failure. Easier
  to reason about and debug; SEC is a potential bottleneck/SPOF.

## Compensating transactions
Must be **idempotent**. The SEC inspects the saga log to determine which steps completed,
which rolled back, and which are pending — then sequences compensations correctly.
Example: billing → compliance → carrier provisioning. Carrier fails after billing ⇒
automatic refund (see [[scenarios/distributed-number-inventory]]).

## Combined with
[[patterns/outbox-pattern]] (reliable event publishing per step) ·
[[patterns/database-per-service]] (each step local to one service) ·
[[concepts/delivery-semantics]] (idempotency keys prevent double-charge on retry).

## Related
[[patterns/dual-write-problem]] · [[scenarios/distributed-number-inventory]]
