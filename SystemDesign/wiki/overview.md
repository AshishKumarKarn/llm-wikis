---
title: Overview
type: overview
tags: [thesis]
sources: [consistent-hashing-primer, system-design, design-patterns, scenario-questions, miscellaneous]
created: 2026-05-15
updated: 2026-05-15
---

# Interview Prep Wiki — Overview

Entry point and evolving thesis. Revised on major ingests. Navigate via [[index]].

## Scope
System design, distributed systems, AWS cloud services, and behavioral/leadership
interview preparation. Organized as concepts, components, patterns, scenario case
studies, comparisons, and behavioral material.

## Working thesis (v6, 6 sources)
The recurring test in system design interviews is **behavior under change and load** —
not steady-state correctness. A placement scheme is judged by what happens when a node
joins or leaves ([[concepts/consistent-hashing]] vs modulo). A caching strategy is judged
by what happens when a popular key expires ([[components/caching]] stampede). A messaging
system is judged by what happens when a consumer lags ([[components/kafka]] rebalance).

The practical toolkit that recurs across design problems is small: **partitioned Kafka**
for fan-out, **Redis** for fast coordination (locks, rate limits, state), **sagas +
idempotency keys** for distributed transactions, **DLQs + jitter** for reliable delivery,
**token bucket in Redis** for rate limiting, **bulkhead isolation** per tenant.
Master these and the specific scenarios become compositions.

The second layer is **service reliability and migration patterns**: Outbox for reliable
event publishing, Circuit Breaker + Retry for resilience, CQRS for read/write separation,
Strangler Fig for safe legacy migration, Canary/Blue-Green for zero-downtime deploys.
These are the "how do you make it production-ready?" answers that separate SA/EM
candidates.

## Map of the territory (6 sources)
- **Concepts:** [[concepts/consistent-hashing]] · [[concepts/horizontal-scaling]] ·
  [[concepts/delivery-semantics]] · [[concepts/hot-key-hot-partition]] ·
  [[concepts/latency-percentiles]] · [[concepts/streaming-protocols]] ·
  [[concepts/twelve-factor-app]] · [[concepts/solid-principles]] · [[concepts/gof-patterns-overview]] ·
  [[concepts/distributed-tracing]] · [[concepts/microservice-security]] · [[concepts/jvm-debugging]] ·
  [[concepts/rest-api-design]] · [[concepts/domain-driven-design]] · [[concepts/event-driven-architecture]] ·
  [[concepts/db-isolation-levels]] · [[concepts/acid-transactions]] · [[concepts/db-indexes]]
- **Components:** [[components/caching]] · [[components/distributed-cache]] ·
  [[components/load-balancer]] · [[components/kafka]] · [[components/redis]] ·
  [[components/kubernetes]] · [[components/elk-stack]] · [[components/object-storage-s3]] ·
  [[components/aws-aurora]] · [[components/aws-lambda]] · [[components/aws-ec2]] ·
  [[components/api-gateway]] · [[components/service-mesh]]
- **Patterns:** [[patterns/data-partitioning]] · [[patterns/n-plus-1-query]] ·
  [[patterns/saga]] · [[patterns/bulkhead]] · [[patterns/circuit-breaker]] · [[patterns/cqrs]] ·
  [[patterns/service-discovery]] · [[patterns/retry-pattern]] · [[patterns/outbox-pattern]] ·
  [[patterns/event-sourcing]] · [[patterns/strangler-fig]] · [[patterns/database-per-service]] ·
  [[patterns/sidecar-pattern]] · [[patterns/api-composition]] · [[patterns/feign-client]] ·
  [[patterns/dual-write-problem]] · [[patterns/deployment-strategies]]
- **Scenarios:** [[scenarios/rate-limiter]] · [[scenarios/high-volume-notification-dispatcher]] ·
  [[scenarios/distributed-number-inventory]] · [[scenarios/multi-tenant-usage-dashboard]] ·
  [[scenarios/follow-me-voice-routing]] · [[scenarios/webhook-delivery-backoff]] ·
  [[scenarios/api-performance-optimization]] · [[scenarios/concurrency-in-microservices]] ·
  [[scenarios/scheduled-jobs-microservices]] · [[scenarios/data-migration-to-cloud]] ·
  [[scenarios/production-incident-response]] · [[scenarios/slow-db-writes]]
- **Comparisons:** [[comparisons/modulo-vs-consistent-hashing]] ·
  [[comparisons/push-vs-pull-messaging]] · [[comparisons/aurora-vs-rds]] ·
  [[comparisons/sharding-vs-partitioning]]
- **Behavioral:** [[behavioral/em-people-management]] · [[behavioral/em-technical-leadership]] ·
  [[behavioral/sdlc-and-delivery]]

## Open threads
- "Dealing With Contention" and "Managing Long Running Tasks" (stub pages in System Design source) —
  flag for web-fill or next ingest.
- ~18 scenario stub pages (p35–54 of Scenario Based Questions.pdf) — question headers only, no content.
- GraphQL, Spring Reactive, Tomcat thread model — documented in Miscellaneous source page, not yet wikified.
- All 5 PDFs fully ingested. Suggested next: Dynamo paper or system design deep-dives.
