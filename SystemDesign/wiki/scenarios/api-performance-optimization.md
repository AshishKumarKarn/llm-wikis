---
title: API Performance Optimization
type: scenario
tags: [performance, latency, caching, database, microservices, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# API Performance Optimization

Structured approach to diagnosing and fixing API latency/throughput issues in a
microservices/cloud environment. The golden rule: **measure before optimizing**
(see [[sources/scenario-questions-study-guide]]).

## Step 1: Identify the bottleneck
Measure first. Tools: distributed tracing (Jaeger/Zipkin), APM (Dynatrace, New Relic),
logs, metrics.
- Response time P95/P99 (see [[concepts/latency-percentiles]])
- DB query time (slow query log, `pg_stat_statements`)
- External API latency
- CPU/memory/IOPS

**Most API performance issues are database-related**, followed by network latency and
inefficient service-to-service communication.

## Step 2: Optimize at each layer

### Code-level
- Avoid per-item loops that call external services → batch instead
- Use efficient data structures (`HashMap` vs nested loops)
- Use async/non-blocking calls (`CompletableFuture`, reactive)

### Database (biggest impact)
- Add missing indexes; remove unused ones (each index slows writes)
- Fix [[patterns/n-plus-1-query]] — use JOIN or batch query
- Fetch only required fields (`SELECT id, name` not `SELECT *`)
- Connection pooling (never open a connection per request)
- Pagination (`GET /users?page=1&size=20`)
- Read replicas for heavy read workloads ([[patterns/cqrs]])

### Caching
- Local in-memory cache for per-instance hot data
- Distributed cache ([[components/redis]]) for cross-instance shared data
- CDN (CloudFront) for static responses

### Network
- GZIP compression on responses
- Reduce payload size: use DTOs, remove unused fields
- HTTP/2 or keep-alive (reuse connections)

### API design
- Avoid chatty APIs: combine `/user → /orders → /payments` into one call
  ([[patterns/api-composition]])
- Pagination and filtering on all list endpoints

### Parallel processing
```java
CompletableFuture<User> user = userService.getAsync(id);
CompletableFuture<List<Order>> orders = orderService.getAsync(id);
CompletableFuture.allOf(user, orders).join();
```
Fan-out reduces total response time when calls are independent.

### Async processing (for heavy work)
Don't block the API thread for slow operations. Use a queue:
`API → Kafka/SQS → Worker → Process`

### Rate limiting and load balancing
- Rate limiting protects from overload and ensures fair usage
- [[components/load-balancer]] distributes traffic across instances

## Senior-level interview angle
"I would first identify bottlenecks using metrics, distributed tracing, and slow-query
logs, then optimize at the specific layer that's actually slow. Most gains come from
fixing DB queries, adding caching, and parallel fan-out. I'd validate each change with
P99 latency metrics before moving to the next."

## Related
[[concepts/latency-percentiles]] · [[patterns/n-plus-1-query]] · [[components/redis]] ·
[[patterns/api-composition]] · [[patterns/cqrs]] · [[concepts/distributed-tracing]]
