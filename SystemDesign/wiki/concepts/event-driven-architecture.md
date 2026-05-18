---
title: Event-Driven Architecture (EDA)
type: concept
tags: [eda, events, kafka, async, microservices, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# Event-Driven Architecture (EDA)

An architectural pattern where services communicate by producing and consuming events
instead of making direct synchronous calls. When a state change occurs, an event is
published; interested services react asynchronously
(see [[sources/miscellaneous-study-guide]]).

## Core flow
```
Producer Service → Event Broker → Consumer Services

Order Service
    ↓ publishes OrderCreated
Kafka Topic
    ↓
  Payment Service    ← reacts independently
  Inventory Service  ← reacts independently
  Notification Service ← reacts independently
```

## Key components

| Component | Role | Examples |
|---|---|---|
| **Producer** | Generates events on state change | Order Service → `OrderCreated` |
| **Event Broker** | Stores and distributes events reliably | Apache Kafka, RabbitMQ, AWS SNS/SQS |
| **Consumer** | Listens to events and reacts | Inventory Service, Notification Service |

An event represents a **state change** — something that happened, not a command.
Events are typically immutable facts: `OrderPlaced`, `PaymentFailed`, `ShipmentDispatched`.

## Benefits vs. synchronous calls

| | EDA (async) | Synchronous (REST/gRPC) |
|---|---|---|
| Coupling | Loose — producer doesn't know consumers | Tight — caller knows the callee |
| Scalability | Consumers scale independently | Coupled capacity |
| Resilience | Consumer down → events queued, not lost | Service down → caller fails |
| Latency | Eventual consistency | Immediate response |

## Challenges
- **Event ordering:** Kafka guarantees order within a partition; across partitions, no global order.
- **Event duplication:** at-least-once delivery → consumers must be **idempotent**.
- **Eventual consistency:** data is eventually in sync, not immediately.
- **Debugging:** distributed flow requires [[concepts/distributed-tracing]].

## EDA and CQRS
EDA is the natural backbone of [[patterns/cqrs]] — command side generates events that
update the read model asynchronously.

## EDA and Event Sourcing
[[patterns/event-sourcing]] stores events as the source of truth. EDA distributes those
events to other services.

## Interview one-liner
"Event-Driven Architecture services communicate through events published to a broker.
This decouples producers from consumers, enables independent scaling, and improves
resilience at the cost of eventual consistency and debugging complexity."

## Related
[[components/kafka]] · [[patterns/event-sourcing]] · [[patterns/cqrs]] ·
[[patterns/outbox-pattern]] (reliable event publishing) · [[patterns/saga]] (cross-service saga via events)
