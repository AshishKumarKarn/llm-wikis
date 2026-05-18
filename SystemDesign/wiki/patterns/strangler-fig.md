---
title: Strangler Fig Pattern
type: pattern
tags: [migration, legacy, microservices, modernization, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Strangler Fig Pattern

Incrementally replaces a legacy system by routing specific functionality to new
implementations, gradually until the old system can be retired. Named after the
strangler fig tree which grows around a host tree and eventually replaces it
(see [[sources/design-patterns-study-guide]]).

## What problem it solves
Big-bang rewrites are high risk, long timelines, and hard to roll back.
Strangler Fig allows **incremental delivery** with the business running throughout.

## How it works
1. **Identify** a small, well-defined feature (e.g., payments, reporting).
2. **Implement** it as a new microservice/module.
3. **Route** only that functionality via API Gateway, reverse proxy, or load balancer.
4. **Repeat** — gradually move more features.
5. **Retire** the monolith once all functionality is migrated.

```
Client
  |
API Gateway
  |-------> New Microservice (payments)
  |-------> Legacy System (everything else)

Over time:
Legacy → Smaller → Gone
```

## Why not rewrite everything at once?
- High risk of failure
- Long timelines before value delivery
- Hard to roll back if something goes wrong
- Business continuity is disrupted

## Real-world examples
- **E-commerce monolith:** Extract Payments → Orders → Inventory one at a time.
- **Bank COBOL system:** New REST services for customer profiles and notifications;
  gateway routes modern traffic to new services while legacy handles core functions.

## Benefits & trade-offs
| Benefit | Trade-off |
|---|---|
| Safe, incremental modernization | Temporary dual-system complexity |
| Minimal downtime | Both systems must be maintained in parallel |
| Continuous business validation | Requires good routing strategy |
| Gradual team learning | |

## When to use
Large legacy system · moving to microservices · downtime is unacceptable ·
business must continue during migration.

## Related
[[components/api-gateway]] (the routing layer) · [[patterns/database-per-service]]
(each new service gets its own DB)
