---
title: Circuit Breaker Pattern
type: pattern
tags: [resilience, microservices, fault-tolerance, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Circuit Breaker Pattern

Prevents cascading failures when a downstream service is down. Wraps calls with a
state machine that short-circuits requests eagerly once a failure threshold is reached,
saving resources for calls likely to fail (see [[sources/design-patterns-study-guide]]).

## States
- **CLOSED** — normal operation; requests pass through; failures counted.
- **OPEN** — failure threshold exceeded; all requests immediately rejected without
  calling the downstream service; timer started.
- **HALF_OPEN** — timer elapsed; a configured number of trial requests are let through
  to test if the downstream has recovered; success → CLOSED, failure → OPEN again.

## Configurable parameters
Failure rate threshold to trip · wait duration in OPEN state · ring buffer size in
HALF_OPEN/CLOSED · custom event listener · custom predicate for what counts as a failure.

## Implementation
Resilience4j (modern, pluggable via Spring Cloud Circuit Breaker abstraction) · Hystrix
(older, Netflix; Spring Cloud Netflix wrapper — tightly coupled to Hystrix).
Spring Cloud Circuit Breaker provides the abstraction so you can swap implementations.

## vs Retry pattern
[[patterns/retry-pattern]] handles transient failures optimistically. Circuit Breaker
handles systemic failures pessimistically — stop hammering a broken service.
Use both: retry first; circuit breaker prevents retry storms.

## Related
[[patterns/retry-pattern]] · [[patterns/bulkhead]] · [[patterns/api-composition]]
