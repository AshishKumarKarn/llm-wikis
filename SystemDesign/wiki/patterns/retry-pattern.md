---
title: Retry Pattern
type: pattern
tags: [resilience, fault-tolerance, microservices, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Retry Pattern

Automatically re-sends a failed request a configured number of times before
declaring permanent failure. Handles transient errors (network blips, brief
unavailability) without surfacing them to the caller
(see [[sources/design-patterns-study-guide]]).

## How it works
1. Initial request fails (network timeout, 503, throttle).
2. Retry logic triggers with a configurable **backoff strategy**.
3. After max retries, error is surfaced (logged, alert, fallback, DLQ).

## Backoff strategies
| Strategy | Behavior | Use case |
|---|---|---|
| Constant | Fixed delay between retries | Simple, low-concurrency |
| Incremental | Linearly increasing delay | Gradual load relief |
| Exponential | Delay doubles each attempt (e.g., 1s → 2s → 4s → 8s) | Standard choice |
| Exponential + Jitter | Exponential + random offset | Prevents retry storms (all clients retrying simultaneously) |

**Jitter** is critical in high-concurrency systems — without it, many services
retry at the same moment and re-amplify the failure.

## Configuration
- Max retry attempts (e.g., 3)
- Max delay cap (e.g., 30s)
- Which exception types trigger retry (vs fatal errors like 400 Bad Request)

## Real-world users
AWS SDKs (DynamoDB, S3), Netflix/Hystrix, Stripe payment processing.

## vs Circuit Breaker
Retry handles **transient** failures optimistically.
[[patterns/circuit-breaker]] handles **systemic** failures pessimistically.
**Use both together:** retry first; circuit breaker prevents retry storms when a
service is truly down.

## Related
[[patterns/circuit-breaker]] · [[patterns/bulkhead]] · [[scenarios/webhook-delivery-backoff]]
(exponential backoff with jitter for SMS gateway retries)
