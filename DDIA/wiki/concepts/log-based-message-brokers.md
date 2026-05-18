---
title: Log-Based Message Brokers
type: concept
chapters: [11]
tags: [stream, kafka, log, messaging]
status: solid
updated: 2026-05-16
---

# Log-Based Message Brokers

## Definition

A hybrid combining a database's **durable storage** with messaging's **low-latency
notification**. A **log** = an append-only sequence of records on disk (the Ch 3 /
Ch 5 log). Producer appends; consumer reads sequentially; at the end, waits for new
appends (`tail -f`-like). *(DDIA Ch 11)*

## How it works

- **Partitioned** for throughput (Ch 6): a topic = a group of partitions; each
  partition is an independent totally-ordered log on a machine. Each message gets a
  monotonically increasing **offset** within its partition (no cross-partition
  order). Kafka, Amazon Kinesis, Twitter DistributedLog — millions of msgs/sec via
  partitioning + replication.
- **Load balancing**: assign whole *partitions* (not individual messages) to a
  consumer group. Downsides: parallelism ≤ partition count; one slow message =
  **head-of-line blocking**.
- **Consumer offsets**: broker only periodically records each consumer's offset
  (everything below = processed) — like a [[single-leader-replication]] log sequence
  number (broker = leader, consumer = follower). On consumer failure, another takes
  the partition from the last offset → some messages may be **reprocessed**.
- **Disk usage**: log split into segments; old segments deleted/archived → a bounded
  **circular buffer** (but huge — e.g. 6 TB / 150 MB/s ≈ 11 h, often days/weeks).
  Throughput is **constant** regardless of retention (always writes to disk) — unlike
  in-memory brokers that slow when spilling.
- **Slow consumers**: a form of buffering; a too-slow consumer eventually misses
  messages, but **only that consumer is affected** (monitor lag, alert). Reads are
  non-destructive → **replay** old messages (start a copy at yesterday's offset) →
  like batch's repeatable derived-data transformation; great for experimentation &
  recovery.

## Related concepts

- [[log-based-vs-amqp-jms-brokers]] · [[event-streams-and-messaging]] ·
  [[change-data-capture]] · [[log-structured-storage]] · [[apache-kafka]]

## Sources

DDIA Ch 11 ("Partitioned Logs").
