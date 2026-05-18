---
title: Apache Kafka
type: system
chapters: [1, 11]
tags: [system, log, stream-processing, messaging, cdc]
status: solid
updated: 2026-05-16
---

# Apache Kafka

A distributed, partitioned, replicated **[[log-based-message-brokers|log-based
message broker]]**. Ch 1 cited it for category blurring (a message queue with
database-like durability). Ch 11 makes it the central example. *(DDIA Ch 1 & 11)*

## How it works (Ch 11)

- Topic = group of **partitions**; each partition an independent append-only log;
  per-partition monotonic **offset**; millions of msgs/sec via partitioning +
  replication. Consumer groups get whole partitions; consumers track offsets.
- Reads are **non-destructive** → replay; bounded but huge disk buffer; constant
  throughput regardless of retention.
- **Log compaction** (keep latest value per key) → the log holds a full DB copy →
  rebuild derived systems from offset 0. The basis for **[[change-data-capture|CDC]]**
  transport and **Kafka Connect** (integrate CDC sources/sinks), Kafka Streams /
  Samza state replication, Druid/Pistachio ingestion.
- **Exactly-once** / transactional messaging (KIP-98) for
  [[stream-fault-tolerance]].

## Related

- [[log-based-message-brokers]] · [[log-based-vs-amqp-jms-brokers]] ·
  [[change-data-capture]] · [[state-streams-immutability]] · [[redis]]
  (the dual example) · [[the-log-kreps]]
