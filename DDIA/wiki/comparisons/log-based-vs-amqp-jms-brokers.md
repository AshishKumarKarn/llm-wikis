---
title: "Log-Based vs. AMQP/JMS Message Brokers"
type: comparison
chapters: [11]
tags: [stream, messaging, comparison]
status: solid
updated: 2026-05-16
---

# Log-Based vs. AMQP/JMS Message Brokers

The two message-broker styles of [[ch11-stream-processing]]. *(DDIA Ch 11 Summary)*

| | AMQP/JMS-style | Log-based |
|---|---|---|
| Delivery unit | individual message → a consumer | whole partition → a consumer node |
| Ack | per-message ack; **message deleted** on ack | periodic **offset** checkpoint; message retained |
| Reads | **destructive** (can't replay) | non-destructive → **replay** old messages |
| Ordering | lost under load-balancing + redelivery | preserved within a partition |
| Parallelism | fine-grained, per-message | coarse: ≤ partition count |
| New consumer | sees only messages after it joined | can read arbitrarily far back |
| Slow message | another consumer takes it | **head-of-line blocking** in the partition |
| Examples | RabbitMQ, ActiveMQ, IBM MQ, Pub/Sub | Kafka, Kinesis, DistributedLog |

## When to use which

- **AMQP/JMS**: messages expensive to process, want per-message parallelism, ordering
  unimportant, no need to replay — e.g. an asynchronous-RPC **task queue**.
- **Log-based**: high throughput, each message fast to process, **ordering matters**,
  want durable replayable streams feeding **derived data systems** (CDC, event
  sourcing, materialized views).

## Related

- [[event-streams-and-messaging]] · [[log-based-message-brokers]] ·
  [[change-data-capture]] · [[apache-kafka]]

## Sources

DDIA Ch 11 ("Logs compared to traditional messaging", Summary).
