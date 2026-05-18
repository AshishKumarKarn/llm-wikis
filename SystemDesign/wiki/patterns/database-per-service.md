---
title: Database Per Service Pattern
type: pattern
tags: [database, microservices, autonomy, patterns]
sources: [design-patterns, system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Database Per Service Pattern

Each microservice owns and controls its own database (or schema). No other service
can access it directly — all data access goes through the service's API
(see [[sources/design-patterns-study-guide]]).

## What it solves
Shared database = hidden coupling: schema changes in one service break others,
independent scaling is impossible, deployments are coupled. Database Per Service
enforces true service autonomy.

## Variants (loosest → strictest isolation)
1. Different **tables** in the same database, no cross-table joins between services.
2. Different **schemas** in the same database instance, schemas not visible to each other.
3. Completely **separate databases** (different instances, different technology).

## How services share data (without sharing DB)
| Approach | Mechanism | Consistency |
|---|---|---|
| API calls | Service A calls Service B's REST/gRPC API | Synchronous, tight-ish coupling |
| Event-driven | Services publish events; others build local read copies | Eventual consistency |

Event-driven is preferred at scale — combined with [[patterns/saga]] and
[[patterns/outbox-pattern]].

```
Order Service  → Orders DB (Postgres)
Payment Service → Payments DB (Postgres)
Inventory Service → Inventory DB (MongoDB)
Search Service → Elasticsearch
```

## Benefits & trade-offs
| Benefit | Trade-off |
|---|---|
| Independent scaling | Data duplication across services |
| Independent deployments | Complex consistency management |
| Technology flexibility (SQL, NoSQL, etc.) | Harder cross-service queries |
| Clear ownership boundaries | No distributed transactions → eventual consistency |

## Shared DB anti-pattern
- ❌ Schema changes break multiple services
- ❌ Hard to scale independently
- ❌ Deployment coupling
- ❌ Ownership ambiguity

## When to use
Building true microservices where teams own services end-to-end and independent
scaling is required.

**Avoid** when the system is small or when strong consistency across entities is
non-negotiable (consider a monolith instead).

## Related
[[patterns/saga]] · [[patterns/outbox-pattern]] · [[patterns/dual-write-problem]] ·
[[patterns/strangler-fig]] (migration path from shared DB)
