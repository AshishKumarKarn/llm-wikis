---
title: Index
type: index
updated: 2026-05-16
---

# Wiki Index

Content catalog. Read this first when answering queries, then drill into relevant pages.

## Overview
- [[overview]] — Evolving thesis; map of the territory

## Sources
- [[sources/consistent-hashing-primer]] — Seed demo on consistent hashing · 1 source
- [[sources/system-design-study-guide]] — 150-page interview study guide (Kafka, Redis, K8s, AWS, 6 design problems) · 2 sources
- [[sources/design-patterns-study-guide]] — 101-page patterns guide (microservices p1–71; GoF+SOLID p72–101 image-only) · 3 sources
- [[sources/scenario-questions-study-guide]] — 99-page scenario Q&A (API perf, concurrency, JVM debugging, data migration) · 4 sources
- [[sources/miscellaneous-study-guide]] — 214-page mixed guide (REST, DDD, EDA, DB isolation, code review) · 5 sources
- [[sources/leadership-study-guide]] — 60-page leadership guide (people management, architecture Q&A, SDLC, Agile) · 6 sources

## Concepts
- [[concepts/consistent-hashing]] — Hash ring + virtual nodes, ~K/N relocation on resize · 2 sources
- [[concepts/horizontal-scaling]] — Scaling out; why stateful distribution is the hard part · 2 sources
- [[concepts/delivery-semantics]] — At-most / at-least / exactly-once; idempotency · 1 source
- [[concepts/hot-key-hot-partition]] — Load imbalance in caches, Kafka, DBs; fixes · 1 source
- [[concepts/latency-percentiles]] — P90/P95/P99; why averages lie · 1 source
- [[concepts/streaming-protocols]] — HLS, DASH, RTMP, RTSP, SRT, WebRTC comparison · 1 source
- [[concepts/twelve-factor-app]] — 12 factors for cloud-native apps; config, stateless, disposability · 1 source
- [[concepts/solid-principles]] — SRP, OCP, LSP, ISP, DIP with examples · 1 source
- [[concepts/gof-patterns-overview]] — All 23 GoF patterns: creational, structural, behavioral · 1 source
- [[concepts/distributed-tracing]] — Traces, spans, OpenTelemetry, Jaeger/Zipkin; sampling best practices · 1 source
- [[concepts/microservice-security]] — OAuth2, mTLS, RBAC, secrets, Zero Trust, CI/CD scanning · 1 source
- [[concepts/jvm-debugging]] — Heap/thread dumps, GC logs; shallow vs retained heap; MAT, fastThread · 1 source
- [[concepts/rest-api-design]] — REST constraints, Richardson Maturity Model, versioning strategies · 1 source
- [[concepts/domain-driven-design]] — Bounded context, entities, aggregates; maps to microservice boundaries · 1 source
- [[concepts/event-driven-architecture]] — Producer-broker-consumer; EDA vs sync; challenges; ties to CQRS/ES · 1 source
- [[concepts/db-isolation-levels]] — 4 anomalies, 4 levels, MVCC, SSI; PostgreSQL vs MySQL vs SQL Server · 1 source
- [[concepts/acid-transactions]] — Atomicity, Consistency, Isolation, Durability; ACID vs BASE · 1 source
- [[concepts/db-indexes]] — Clustered, non-clustered, composite; leftmost prefix rule; when to add/remove · 1 source

## Components
- [[components/caching]] — Strategies, eviction, consistency, stampede, hot-key, penetration, big-key · 2 sources
- [[components/distributed-cache]] — Cache sharded across nodes; node-churn problem · 2 sources
- [[components/load-balancer]] — L4 vs L7, algorithms, SSL termination, caveats · 1 source
- [[components/kafka]] — Partitioned append-only log; delivery, performance, production issues · 1 source
- [[components/redis]] — Multi-tool: cache, lock, leaderboard, rate-limiter, pubsub, streams · 1 source
- [[components/kubernetes]] — Architecture, workloads, networking, probes, storage · 1 source
- [[components/elk-stack]] — Collect→Store→Visualize; ELK vs Kafka pipeline · 1 source
- [[components/object-storage-s3]] — 11 nines durability, storage classes, SAFE DATA · 1 source
- [[components/aws-aurora]] — Compute/storage separation, 6-copy quorum, failover, Serverless v2 · 1 source
- [[components/aws-lambda]] — Serverless event-driven compute; cold start; RDS Proxy caveat · 1 source
- [[components/aws-ec2]] — Instance families, purchasing, storage types, Auto Scaling Groups · 1 source
- [[components/api-gateway]] — Single entry point; routing, auth, rate-limiting, BFF pattern · 1 source
- [[components/service-mesh]] — East-west traffic; Istio/Linkerd; mTLS, tracing, canary via sidecars · 1 source

## Patterns
- [[patterns/data-partitioning]] — Hash/range/directory strategies; Kafka + DB shard key · 2 sources
- [[patterns/n-plus-1-query]] — Anti-pattern; fix with JOIN, batch query, ORM eager loading · 1 source
- [[patterns/saga]] — Distributed transactions via compensation; choreography vs orchestration · 2 sources
- [[patterns/bulkhead]] — Isolate resources per tenant; prevent cascading failures · 2 sources
- [[patterns/circuit-breaker]] — CLOSED/OPEN/HALF_OPEN states; Resilience4j; stop retry storms · 1 source
- [[patterns/cqrs]] — Separate read/write models; fast denormalized read store · 1 source
- [[patterns/service-discovery]] — Client-side vs server-side; self-registration vs third-party · 1 source
- [[patterns/retry-pattern]] — Exponential backoff + jitter; vs circuit breaker · 1 source
- [[patterns/outbox-pattern]] — Atomic event + data in same transaction; CDC vs polling publisher · 1 source
- [[patterns/event-sourcing]] — Immutable event log as source of truth; projections for reads · 1 source
- [[patterns/strangler-fig]] — Incremental legacy migration via routing; no big-bang rewrite · 1 source
- [[patterns/database-per-service]] — Service autonomy; no shared DB; event-driven data sharing · 2 sources
- [[patterns/sidecar-pattern]] — Helper container for infra concerns; foundation of service mesh · 1 source
- [[patterns/api-composition]] — Server-side fan-out + aggregate; BFF; partial failure handling · 1 source
- [[patterns/feign-client]] — Declarative Spring Cloud HTTP client; Eureka + Ribbon integration · 1 source
- [[patterns/dual-write-problem]] — DB + Kafka atomicity failure; Outbox/CDC as canonical fix · 1 source
- [[patterns/deployment-strategies]] — Recreate→Rolling→Blue-Green→Canary→Feature Flags risk ladder · 1 source

## Scenarios
- [[scenarios/rate-limiter]] — Token bucket in Redis; global consistency trade-offs · 1 source
- [[scenarios/high-volume-notification-dispatcher]] — 10M notifications in 5 min; Kafka fan-out, SMS gateway rate-limiting · 1 source
- [[scenarios/distributed-number-inventory]] — Redis NX lock, saga purchase, ES search, country sharding · 1 source
- [[scenarios/multi-tenant-usage-dashboard]] — Kafka push + 3-tier OLAP; isolation from message path · 1 source
- [[scenarios/follow-me-voice-routing]] — Redis call state, virtual threads, dual-clock enforcement · 1 source
- [[scenarios/webhook-delivery-backoff]] — Jitter backoff, bulkhead isolation, HMAC signing, DLQ · 1 source
- [[scenarios/api-performance-optimization]] — 10-layer optimization checklist; measure first; DB is usually the bottleneck · 1 source
- [[scenarios/concurrency-in-microservices]] — OCC, pessimistic lock, distributed lock, idempotency, saga; 5 real problems · 1 source
- [[scenarios/scheduled-jobs-microservices]] — Leader election, ShedLock, K8s CronJob, Temporal · 1 source
- [[scenarios/data-migration-to-cloud]] — DMS + CDC, DataSync, Snowball; phased migration, zero-downtime cutover · 1 source
- [[scenarios/production-incident-response]] — C-D-F-P playbook; cascading failure isolation; blameless postmortem · 1 source
- [[scenarios/slow-db-writes]] — 10-step checklist: indexes, locking, batching, async, sharding, CQRS · 1 source

## Comparisons
- [[comparisons/modulo-vs-consistent-hashing]] — Full remap vs ~K/N relocation · 2 sources
- [[comparisons/push-vs-pull-messaging]] — Producer-side push vs central polling; Kafka nuance · 1 source
- [[comparisons/aurora-vs-rds]] — Storage architecture, failover, Serverless v2, RDS Proxy trap · 1 source
- [[comparisons/sharding-vs-partitioning]] — Scope, goals, when to use; partition types; shard key pitfalls · 2 sources

## Behavioral
- [[behavioral/em-people-management]] — Performance Q&A; cross-team conflict; underperformer steps; goals framework · 1 source
- [[behavioral/em-technical-leadership]] — Architecture decisions, tech debt, production outage, observability; SA/EM themes · 1 source
- [[behavioral/sdlc-and-delivery]] — SDLC phases and model comparison; Agile principles, Scrum/Kanban terms, interview Q&A · 1 source
