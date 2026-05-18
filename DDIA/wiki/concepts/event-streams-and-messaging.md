---
title: Event Streams & Messaging Systems
type: concept
chapters: [11]
tags: [stream, messaging, pub-sub]
status: solid
updated: 2026-05-16
---

# Event Streams & Messaging Systems

## Definition

An **event** = a small, self-contained, **immutable** object recording something that
happened at a point in time (usually with a timestamp). Generated once by a
**producer** (publisher), processed by multiple **consumers** (subscribers); related
events grouped into a **topic/stream**. The streaming counterpart of a batch record.
*(DDIA Ch 11)*

## Why a messaging system (not polling)

A file/DB + consumer polling works (≈ daily batch) but polling is expensive at low
latency. Better: **notify** consumers of new events. DB triggers are limited →
specialized **messaging systems** (publish/subscribe; multiple producers & consumers
per topic, unlike a 1:1 Unix pipe/TCP).

Two differentiating questions:

1. **Producers outpace consumers?** Drop messages / buffer in a queue / **backpressure**
   (flow control — Unix pipes & TCP). If buffering: does it spill to disk, and how
   does that affect performance?
2. **Nodes crash?** Durability needs disk write and/or replication (a cost). Whether
   loss is acceptable is app-dependent (a missed metric vs. a miscounted event).

Batch's strong reliability (retry, discard partial output → exactly-once) is the goal
to recover in streaming ([[stream-fault-tolerance]]).

## Direct messaging vs. message brokers

- **Direct** (UDP multicast for stock feeds, ZeroMQ/nanomsg, StatsD/UDP, webhooks):
  works but app must handle loss; assumes producers & consumers constantly online —
  an offline consumer misses messages.
- **Message broker** (= a database optimized for message streams): producers/
  consumers are clients; centralizes durability; tolerates clients coming/going;
  generally unbounded queueing; consumers **asynchronous**.

## Broker patterns

- **Multiple consumers**: **load balancing** (each message → one consumer, share
  work; AMQP shared queue / JMS shared subscription) and **fan-out** (each message →
  all consumers; JMS topic / AMQP exchange bindings) — combinable.
- **Acknowledgments & redelivery**: consumer must ack; no ack → redeliver to another
  consumer. **Load balancing + redelivery reorders messages** (m3 redelivered after
  m4) — use a queue per consumer if order matters. (Lost-ack double-processing needs
  an atomic commit — see [[distributed-transactions-xa]].)

Traditional brokers: JMS/AMQP — RabbitMQ, ActiveMQ, IBM MQ, Azure Service Bus,
Google Pub/Sub. The durable-replayable alternative → [[log-based-message-brokers]].

## Related concepts

- [[log-based-message-brokers]] · [[log-based-vs-amqp-jms-brokers]] ·
  [[message-passing-dataflow]] (Ch 4) · [[stream-fault-tolerance]]

## Sources

DDIA Ch 11 ("Transmitting Event Streams").
