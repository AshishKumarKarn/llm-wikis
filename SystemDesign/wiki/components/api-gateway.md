---
title: API Gateway
type: component
tags: [api-gateway, microservices, routing, security, components]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# API Gateway

Single entry point between clients and microservices that handles routing, security,
and cross-cutting concerns. Clients talk to the gateway, not individual services
(see [[sources/design-patterns-study-guide]]).

## What it solves
Without gateway: clients call many services → complex client logic, duplicated
security, no central observability. With gateway: one address, consistent behavior.

## Responsibilities
- Request routing
- Authentication & authorization (JWT validation, OAuth)
- Rate limiting & throttling
- DDoS protection
- SSL/TLS termination
- API composition (fan-out + aggregate)
- Logging & distributed tracing
- Response caching
- Response header/body transformation

## Topology options
| Type | Description |
|---|---|
| Single gateway | One gateway for all clients — simple, can become a monolith |
| BFF (Backend for Frontend) | One gateway per client type (Web, Mobile, Partner API) — each optimizes for its consumer |

Netflix uses BFF: different gateway tailored for web, mobile, and TV clients.

## Trade-offs
| Pro | Con |
|---|---|
| Single entry point | Potential SPOF (needs HA) |
| Centralized security | Added latency (extra hop) |
| Simplified clients | Can become a monolith if overloaded with logic |
| Better observability | |

## API Gateway vs API Composition
Often confused — used together. Gateway handles **cross-cutting** concerns;
[[patterns/api-composition]] handles **data aggregation** from multiple services.

## When to use
Multiple microservices, multiple client types, centralized security needed.
**Avoid** for very small systems or single-service architectures.

## Related
[[patterns/api-composition]] · [[patterns/service-discovery]] ·
[[patterns/sidecar-pattern]] (sidecar proxies sit below the gateway per-service) ·
[[components/service-mesh]] (mesh handles east-west; gateway handles north-south)
