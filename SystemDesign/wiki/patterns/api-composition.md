---
title: API Composition Pattern
type: pattern
tags: [api-composition, aggregation, microservices, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# API Composition Pattern

A service or layer calls multiple backend microservices, combines their responses, and
returns a single response to the client. The client sees one API, not many
(see [[sources/design-patterns-study-guide]]).

## What problem it solves
Without composition, clients must call N services → N round trips, complex client
logic, tight coupling to service structure. API Composition aggregates server-side.

## Simple structure
```
Client
  |
API Composer
  |----> Order Service
  |----> Payment Service
  |----> Inventory Service
  |
Combined Response
```

## Where composition lives
- Dedicated Composition Service
- [[components/api-gateway]] (handles it as one of its responsibilities)
- Backend-for-Frontend (BFF)

## Failure handling
The composer must handle:
- **Timeouts** per downstream call
- **Partial responses** (return what's available; degrade gracefully)
- **Fallbacks** — combined with [[patterns/circuit-breaker]]

## API Composition vs Client-Side Composition
| | API Composition | Client Composition |
|---|---|---|
| Where | Server-side | Client-side |
| Network calls | Fewer (server-to-server) | Many (client-to-service) |
| Control | Better abstraction | More client complexity |

## API Composition vs CQRS
| API Composition | CQRS |
|---|---|
| Aggregates API responses | Separates read/write models |
| Orchestration pattern | Architectural pattern |
| Can be used together | |

## Benefits & trade-offs
| Benefit | Trade-off |
|---|---|
| Simple client contract | Extra network hop |
| Reduced client-to-service chatter | Potential bottleneck |
| Centralized orchestration | Partial failure complexity |

## When to use
UI needs data from multiple services · clean client contract desired ·
latency can be optimized server-side with parallel fan-out.

**Avoid** when a single service can serve the request, or extreme low-latency
is required.

## Related
[[components/api-gateway]] · [[patterns/circuit-breaker]] · [[patterns/cqrs]]
