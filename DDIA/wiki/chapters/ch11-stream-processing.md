---
title: "Ch 11 — Stream Processing"
type: chapter
chapters: [11]
tags: [stream, messaging, kafka, cdc, event-sourcing, derived-data]
status: solid
updated: 2026-05-16
---

# Ch 11 — Stream Processing

## One-paragraph thesis

Batch processing assumes **bounded** input; real data is **unbounded** (arrives
continually). Stream processing abandons fixed time slices and processes each
**event** as it happens — the streaming counterpart of batch, with message
brokers/event logs playing the role of the filesystem. The chapter's arc: how streams
are transmitted (messaging systems, **log-based brokers**); the deep connection
between **databases and streams** (a replication log *is* a stream — CDC, event
sourcing, the log↔state duality, immutability); and how to process streams (CEP,
analytics, materialized views; reasoning about **event vs. processing time**; the
three **stream joins**; **exactly-once** fault tolerance). *(DDIA Ch 11; Part III
derived data.)*

## Key ideas

- **[[event-streams-and-messaging]]** — events, producers/consumers/topics; direct
  vs. broker messaging; load-balancing vs. fan-out; acks & redelivery reordering.
- **[[log-based-message-brokers]]** — partitioned append-only log; offsets;
  non-destructive reads → **replay**; large disk buffer; one-partition-per-consumer.
- **[[log-based-vs-amqp-jms-brokers]]** — when each wins.
- **[[change-data-capture]]** — make one DB the leader, derived systems followers;
  the **dual-writes** race it fixes; snapshots, log compaction.
- **[[event-sourcing]]** — store immutable app-level events; commands vs. events;
  derive state by replay.
- **[[state-streams-immutability]]** — "the log is the truth, the DB is a cache";
  changelog↔state duality; advantages of immutable events; **CQRS**; limits.
- **[[stream-processing-uses]]** — CEP, stream analytics, materialized views, search
  on streams; messaging/RPC crossover.
- **[[reasoning-about-time-in-streams]]** — event vs. processing time; stragglers;
  window types (tumbling/hopping/sliding/session).
- **[[stream-joins]]** — stream-stream / stream-table / table-table;
  time-dependence & slowly changing dimensions.
- **[[stream-fault-tolerance]]** — exactly-once ("effectively-once"); microbatch/
  checkpoint; atomic commit; idempotence; rebuilding state.

## Concepts introduced

- [[event-streams-and-messaging]] · [[log-based-message-brokers]] ·
  [[change-data-capture]] · [[event-sourcing]] · [[state-streams-immutability]]
- [[stream-processing-uses]] · [[reasoning-about-time-in-streams]] ·
  [[stream-joins]] · [[stream-fault-tolerance]]

## Comparisons introduced

- [[log-based-vs-amqp-jms-brokers]] · [[cdc-vs-event-sourcing]]

## Systems / papers referenced

- [[apache-kafka]] (now expanded), [[redis]], Kinesis/DistributedLog,
  RabbitMQ/ActiveMQ (AMQP/JMS), [[apache-flink]], [[apache-samza]], Spark Streaming,
  Storm; Druid; Debezium/Bottled Water/Maxwell (CDC); Event Store; Elasticsearch
  percolator
- [[the-log-kreps]] (Kreps, "The Log" 2013), Dataflow Model paper

## Trade-offs & tensions

- Drop / buffer / backpressure when consumers lag; durability cost.
- AMQP/JMS (per-message, expensive parallel work, ordering loose) vs. log-based
  (partition parallelism, replayable, ordering preserved, head-of-line blocking).
- CDC (low-level, app-agnostic) vs. event sourcing (app-level intent, no naive log
  compaction).
- Immutability power vs. limits (churn, fragmentation, GDPR deletion / excision).
- Event time (deterministic, straggler problem) vs. processing time (simple, wrong
  under lag).
- Exactly-once via distributed transactions vs. idempotence (needs deterministic
  replay + fencing).

## Connections to other chapters

- Log reuses Ch 3 [[log-structured-storage]] & Ch 5 [[replication-log-implementations]]
  / [[change-data-capture]] (Ch 5 stub now expanded); state-machine replication ↔
  [[total-order-broadcast]] (Ch 9); consumer offset ↔ [[single-leader-replication]]
  log sequence number.
- Dual-writes race ↔ [[happens-before-and-concurrency]] / [[two-phase-commit]];
  exactly-once ↔ [[distributed-transactions-xa]]; fencing ↔ [[fencing-tokens]].
- Materialized views ↔ [[materialized-views-and-data-cubes]]; CQRS ↔
  [[normalization-and-denormalization]]; Twitter timeline ↔ [[load-parameters]];
  derived data ↔ [[systems-of-record-and-derived-data]]; window/skew ↔ Ch 10.

## Open questions / things to revisit

- Multi-partition processing & "killing lambda"; the unbundled database (Ch 12)?
- Truly deleting immutable data (privacy) — Ch 12 legislation.
