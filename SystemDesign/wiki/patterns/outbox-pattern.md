---
title: Outbox Pattern
type: pattern
tags: [outbox, dual-write, event-driven, consistency, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Outbox Pattern

Solves the [[patterns/dual-write-problem]] by writing domain data **and** the
event into the same DB transaction, then publishing the event asynchronously
from an outbox table (see [[sources/design-patterns-study-guide]]).

## The problem it solves
Without Outbox:
1. `UPDATE orders` ✅
2. `PUBLISH to Kafka` ❌ (network failure)

→ DB is updated; downstream services never hear about it → inconsistency.

## How it works
```
DB Transaction:
  INSERT INTO orders ...
  INSERT INTO outbox_events (type, payload) VALUES ('OrderCreated', {...})
COMMIT  ← atomic; either both succeed or both fail

Separate publisher process:
  SELECT unprocessed rows FROM outbox_events
  Publish to Kafka
  Mark as processed
```

## Two publishing approaches
| Approach | Mechanism | Trade-offs |
|---|---|---|
| Polling publisher | Background job polls outbox table | Simple; slight delay |
| CDC (Change Data Capture) | Debezium reads DB transaction log | Near real-time; infra complexity; event format tied to DB schema |

Debezium (CDC) is the preferred production approach for low-latency systems.

## Delivery guarantee
At-least-once delivery → **consumers must be idempotent**. Duplicate events
are possible (publisher crash after publish, before marking processed).

## Trade-offs
- Extra `outbox_events` table + storage overhead
- Slight latency vs direct publish
- More operational components (publisher process or Debezium)
- Requires idempotent consumers

## When to use
Any time you publish events after a DB write and data consistency is critical
(Kafka, RabbitMQ, SNS). Used by Netflix, Uber, AWS in serious event-driven systems.

## Related
[[patterns/dual-write-problem]] · [[patterns/saga]] (per-step reliable publishing) ·
[[concepts/delivery-semantics]] · [[patterns/cqrs]] (event feeds the read model)
