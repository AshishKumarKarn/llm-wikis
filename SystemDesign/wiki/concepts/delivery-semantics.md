---
title: Delivery Semantics
type: concept
tags: [messaging, kafka, idempotency, distributed-systems]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Delivery Semantics

How a messaging system handles the guarantee that a message is received and processed
(see [[sources/system-design-study-guide]]).

## The three semantics
| Semantic | Behavior | When to use |
|---|---|---|
| **At-most-once** | Offset committed before processing; message may be lost on failure | Low-value events where loss is acceptable |
| **At-least-once** | Message reprocessed if consumer crashes before committing offset; duplicates possible | **Preferred default** — needs idempotent consumers |
| **Exactly-once** | No loss, no duplicates | Kafka-to-Kafka via Streams API; financial transactions |

## Achieving each in Kafka
- **At-most-once:** auto-commit immediately on receive.
- **At-least-once:** manual commit after processing; `enable.idempotence=true` on producer
  + `acks=all` prevents producer duplicates.
- **Exactly-once:** Kafka Streams transactional API; very complex outside Kafka ecosystem.

## Idempotent consumers
The practical answer for at-least-once: use a business key or `(broadcast_id + user_id)`
as a deduplication key; check-and-skip before processing. See
[[scenarios/high-volume-notification-dispatcher]].

## Idempotency keys in distributed sagas
Every saga step should carry an idempotency key to prevent double-charging on retry.
See [[scenarios/distributed-number-inventory]], [[patterns/saga]].

## Related
[[components/kafka]] · [[patterns/saga]] · [[scenarios/webhook-delivery-backoff]]
