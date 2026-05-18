---
title: CQRS (Command Query Responsibility Segregation)
type: pattern
tags: [cqrs, microservices, read-write-separation, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# CQRS — Command Query Responsibility Segregation

Separates read (Query) and write (Command) operations into different models instead of
one unified model for both (see [[sources/design-patterns-study-guide]]).

## Core idea
**Commands** → modify data (create/update/delete); validated, transactional models.
**Queries** → read data; denormalized, fast-read models (Redis, Elasticsearch, MongoDB).
The model used to **update** data is never the same as the model used to **read** it.

## Write process
Command arrives → validate business rules → update write DB → generate event
("CREW_ASSIGNED", "FLIGHT_DELAYED") → publish.

## Read process
Consume event → update read model (optimized for the UI: Redis, ES, denormalized) →
UI reads from this fast store, not the write DB.

## Benefits
Read and write workloads scale independently · different data models per use case ·
reads become fast because data is shaped for the UI · natural fit for event-driven /
microservices architectures.

## When to use
Read/write workloads differ drastically · real-time dashboards · complex business logic ·
event-driven microservices.
**Don't** use for small simple systems — overhead is significant.

## Often combined with
[[patterns/event-sourcing]] (events as the write model) ·
[[scenarios/multi-tenant-usage-dashboard]] (read model = Druid/ClickHouse hot store)

## Related
[[patterns/event-sourcing]] · [[patterns/outbox-pattern]] ·
[[components/redis]] (as read model)
