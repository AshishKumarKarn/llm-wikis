# Wiki Log

Append-only. Each entry header is parseable: `grep "^## \[" log.md | tail -5`.

## [2026-05-15] init | Wiki bootstrapped
Created schema (CLAUDE.md), index.md, log.md, overview.md, and the raw/ + wiki/ directory
structure. Domain: system design & distributed systems. Git repo initialized.

## [2026-05-15] ingest | Primer on Consistent Hashing
Seed source to demonstrate the ingest pipeline end-to-end.
Pages created: sources/consistent-hashing-primer, concepts/consistent-hashing,
concepts/horizontal-scaling, components/distributed-cache, patterns/data-partitioning,
comparisons/modulo-vs-consistent-hashing. Updated: overview, index.
Contradictions: none (first source). Suggested next: Dynamo paper, "Design a distributed cache".

## [2026-05-16] ingest | Leadership Study Guide (60 pages)
Full ingest of Leadership.pdf — personal interview study notes, Feb–May 2026.
Pages created (4 new): sources/leadership-study-guide ·
behavioral/em-people-management, em-technical-leadership, sdlc-and-delivery.
Updated: index, log, overview.
Contradictions: none. All 4 sections covered: Team Management (p1–48),
Goals Settings (p49–52), SDLC (p53–56), Agile (p57–60).
Suggested next: Dynamo paper or GraphQL (noted gap in Miscellaneous.pdf).

## [2026-05-16] ingest | Miscellaneous Study Guide (214 pages)
Full ingest of Miscellaneous.pdf — personal interview study notes, Mar–May 2026.
Pages created (8 new): sources/miscellaneous-study-guide ·
concepts/rest-api-design, domain-driven-design, event-driven-architecture,
db-isolation-levels, acid-transactions, db-indexes ·
comparisons/sharding-vs-partitioning.
Updated: index, log, overview.
Contradictions: none. Gaps: many headers-only pages (Rank vs Dense Rank, DELETE vs
TRUNCATE, Vacuum, Schema Validators); Code Review exercises too Spring-specific to
wiki; GraphQL, Spring Reactive, Tomcat thread model mapped in source page for future ingest.
Suggested next: Leadership.pdf.

## [2026-05-15] ingest | Scenario Based Questions Study Guide (99 pages)
Full ingest of Scenario Based Questions.pdf — personal interview study notes, Mar–May 2026.
Pages created (10 new): sources/scenario-questions-study-guide ·
scenarios/api-performance-optimization, concurrency-in-microservices,
scheduled-jobs-microservices, data-migration-to-cloud, production-incident-response,
slow-db-writes · concepts/distributed-tracing, microservice-security, jvm-debugging.
Updated: index, log, overview.
Contradictions: none. Gaps: ~18 question-header stubs (p35–54) without answer content —
documented in source page. Redis race condition material folded into components/redis.
Suggested next: Miscellaneous.pdf.

## [2026-05-15] ingest | Design Patterns Study Guide (101 pages)
Full ingest of Design Patterns.pdf — personal interview study notes, Jan–Mar 2026.
Pages created (18 new): sources/design-patterns-study-guide · patterns/circuit-breaker,
cqrs, service-discovery, retry-pattern, outbox-pattern, event-sourcing, strangler-fig,
database-per-service, sidecar-pattern, api-composition, feign-client, dual-write-problem,
deployment-strategies · components/api-gateway, service-mesh · concepts/twelve-factor-app,
solid-principles, gof-patterns-overview.
Pages enriched (2): patterns/saga, patterns/bulkhead (stubs → full content).
Updated: index, log.
Contradictions: none. Gaps: SOLID + GoF (p72–101) are image-only — wiki pages written
from general knowledge with clear attribution note.
Suggested next: Scenario Based Questions.pdf.

## [2026-05-15] ingest | System Design Study Guide (150 pages)
Full ingest of System Design.pdf — personal interview study notes, Jan–May 2026.
Pages created (26 new): sources/system-design-study-guide · components/caching,
load-balancer, kafka, redis, kubernetes, elk-stack, object-storage-s3, aws-aurora,
aws-lambda, aws-ec2 · concepts/delivery-semantics, hot-key-hot-partition,
latency-percentiles, streaming-protocols · patterns/n-plus-1-query, saga (stub),
bulkhead (stub) · scenarios/rate-limiter, high-volume-notification-dispatcher,
distributed-number-inventory, multi-tenant-usage-dashboard, follow-me-voice-routing,
webhook-delivery-backoff · comparisons/push-vs-pull-messaging, aurora-vs-rds.
Pages updated (6): concepts/consistent-hashing, horizontal-scaling · patterns/data-partitioning ·
components/distributed-cache · overview · index.
Contradictions: none. Gaps: "Dealing With Contention" + "Managing Long Running Tasks"
(p149-150) are stubs in source — no body in PDF export.
Suggested next: Design Patterns.pdf (will enrich saga/bulkhead patterns).
Seed source to demonstrate the ingest pipeline end-to-end.
Pages created: sources/consistent-hashing-primer, concepts/consistent-hashing,
concepts/horizontal-scaling, components/distributed-cache, patterns/data-partitioning,
comparisons/modulo-vs-consistent-hashing. Updated: overview, index.
Contradictions: none (first source). Suggested next: Dynamo paper, "Design a distributed cache".
