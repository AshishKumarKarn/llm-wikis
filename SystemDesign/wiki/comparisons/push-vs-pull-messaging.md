---
title: "Push vs Pull Messaging"
type: comparison
tags: [kafka, messaging, observability, architecture]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Push vs Pull Messaging

Recurring trade-off in the design problems
(see [[sources/system-design-study-guide]]).

## Precise definitions
The debate is about the **producer/emission side**, not the consumer internals.
- **Push:** microservices proactively emit events the moment something happens.
- **Pull:** a central collector periodically reaches out and scrapes/queries services.

| Dimension | Push (Kafka producer) | Pull (ELK/Logstash polling) |
|---|---|---|
| Latency floor | Milliseconds (event emitted immediately) | Bounded by polling interval (30–60s typical) |
| Sub-second drill-downs | ✅ possible | ❌ impossible by design |
| Coupling | Producers must know a broker exists | Central collector decoupled from producers |
| Failure behavior | Fire-and-forget; drop events not messages | Collector failure = gap in coverage |

## The Kafka nuance
Kafka consumers use a **pull** model internally (they poll brokers). But Kafka Streams
achieves near-real-time because: (1) producers **push** events immediately on each API
call; (2) consumers poll at millisecond intervals. The sub-second latency advantage over
ELK comes from producer-side push. See [[components/kafka]], [[components/elk-stack]].

## Applied in
[[scenarios/multi-tenant-usage-dashboard]] (push via Kafka vs pull via ELK) ·
[[scenarios/webhook-delivery-backoff]] (push-only delivery vs pull-based polling)
