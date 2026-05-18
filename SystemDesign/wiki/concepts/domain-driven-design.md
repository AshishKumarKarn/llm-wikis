---
title: Domain Driven Design (DDD)
type: concept
tags: [ddd, bounded-context, aggregate, microservices, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# Domain Driven Design (DDD)

Software design centered around the business domain and business rules. Provides
vocabulary and structure for modeling complex systems and maps directly to
microservice boundaries (see [[sources/miscellaneous-study-guide]]).

## Core building blocks

### Bounded Context
A boundary where a particular domain model applies — the key unit for microservice design.
Each bounded context has its own database, models, and business rules.
```
Booking Context → Booking Service → Booking DB
Payment Context → Payment Service → Payment DB
Inventory Context → Inventory Service → Inventory DB
```
**Bounded Context = Microservice boundary.** This is why DDD matters for microservices.

### Entity
An object with identity and lifecycle. Even if attributes change, the identity (ID) remains the same.
Examples: `Order`, `Customer`, `Flight`

### Value Object
An object defined entirely by its values; no identity. Immutable.
If values change, it becomes a new object.
Examples: `Address`, `Money`, `DateRange`

### Aggregate
A cluster of domain objects (entities + value objects) treated as a single unit with one
**Aggregate Root** — the only entry point for external access.
```
Order (Aggregate Root)
  └─ OrderItems
  └─ PaymentDetails
```
Rule: external code can only reference the Aggregate Root, never inner objects directly.

### Domain Service
Business logic that doesn't naturally belong to any entity.
Examples: `PaymentProcessingService`, `FlightPricingService`

## Why DDD for microservices
DDD provides the discipline to:
1. Define **where service boundaries belong** (bounded context = service).
2. Prevent cross-service data coupling (each context has its own model).
3. Keep business logic in the domain layer, not scattered in controllers.

**Common mistake:** treating DDD as optional complexity. At SA/EM level, expect to justify
service boundaries using bounded context reasoning.

## Interview one-liner
"Domain-Driven Design models software around business concepts using bounded contexts
to enforce service boundaries, entities for identity-carrying objects, value objects for
immutable concepts, and aggregates as transaction boundaries."

## Related
[[patterns/database-per-service]] (each bounded context owns its DB) ·
[[patterns/saga]] (cross-context transactions via compensation) ·
[[concepts/event-driven-architecture]] (events cross context boundaries)
