---
title: "Source: Miscellaneous Study Guide"
type: source
tags: [rest, ddd, eda, database, concurrency, cloud, microservices]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# Source: Miscellaneous Study Guide

**Origin:** `raw/Miscellaneous.pdf` · 214 pages · personal study notes (OneNote export),
Mar–May 2026 · interview prep (SA / EM / senior engineer focus).

## Summary
Three sections: (1) Cloud and Microservice (p1–160); (2) Code Review (p161–181);
(3) Databases (p182–214).

## Section index → wiki pages

### Cloud and Microservice (p1–160)
- REST Principles + Richardson Maturity Model (p1–6) → [[concepts/rest-api-design]]
- REST API Versioning (p7–9) → [[concepts/rest-api-design]]
- API Layering (p10–14) → [[components/api-gateway]]
- OpenAPI / Swagger (p15–18) → mentioned in [[concepts/rest-api-design]]
- API Mocking / Virtualization (p19–21) → (not mapped; dev tooling)
- Filtering and Mediation (p22–23) → (not mapped)
- Spring Reactive Programming (p24–28) → (not mapped; Spring-specific)
- Aggregator Pattern (p28–30) → [[patterns/api-composition]]
- Domain Driven Design (p30–32) → [[concepts/domain-driven-design]]
- Event Driven Architecture (p33–35) → [[concepts/event-driven-architecture]]
- GraphQL (p36–45) → [[comparisons/graphql-vs-rest]]
- Messaging patterns (p46–100) → partially covered in [[components/kafka]]
- Distributed Locking deep-dive (p58–64) → [[components/redis]] (SET NX, Redlock)
- Tomcat Thread Model (p119–122) → [[concepts/tomcat-thread-model]]
- Virtual Threads / Java (p123+) → (not mapped; Java-specific)

### Code Review (p161–181)
- Exercise 1: DB transaction + external API call anti-pattern → [[concepts/code-review-patterns]]
- Exercise 2: Leaky SMS Batch Processor → [[concepts/code-review-patterns]]
- Common Spring Boot anti-patterns (field injection, ForkJoinPool misuse, no backpressure)

### Databases (p182–214)
- Database Partition Types (p182–183) → [[patterns/data-partitioning]]
- Database Index Types: Clustered, Non-Clustered, Composite (p184–185) → [[concepts/db-indexes]]
- Database Normalization (p186–189) → [[concepts/db-normalization]]
- ACID Properties (p190–193) → [[concepts/acid-transactions]]
- Sharding vs Partitioning (p193–194) → [[comparisons/sharding-vs-partitioning]]
- Materialized Views (p195–197) → [[concepts/db-materialized-views]]
- Views vs Tables (p198–200) → [[concepts/db-materialized-views]]
- Local Transactions (p201–202) → [[concepts/acid-transactions]]
- Rank vs Dense Rank (p203) → (header only)
- Delete vs Truncate (p204) → (header only)
- Write Skew (p205) → [[concepts/db-isolation-levels]]
- MVCC (p206) → [[concepts/db-isolation-levels]]
- Isolation Levels deep-dive (p207–213) → [[concepts/db-isolation-levels]]
- PostgreSQL Vacuum (p214) → (header only)

## Key takeaways
1. Isolation levels are a spectrum — pick the weakest level that preserves correctness invariants.
2. MVCC eliminates reader-writer blocking; PostgreSQL SSI is best-in-class for SERIALIZABLE.
3. DDD bounded contexts map 1:1 to microservice boundaries.
4. Code review red flag: DB transaction + external API call in same unit of work.
5. GraphQL: one endpoint, client shapes the query — use for mobile/BFF where bandwidth matters.

## Gaps (headers only, no answer body)
- Rank vs Dense Rank (p203)
- Delete vs Truncate (p204)
- PostgreSQL Vacuum (p214)
- Schema Validators (p65–67) — SEO tooling, not interview relevant
