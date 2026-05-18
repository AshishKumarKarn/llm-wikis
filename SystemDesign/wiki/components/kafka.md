---
title: Apache Kafka
type: component
tags: [kafka, messaging, streaming, partitioning, distributed-systems]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Apache Kafka

Distributed, append-only event log usable as a message queue or stream processor
(see [[sources/system-design-study-guide]]).

## Core concepts
Cluster = many **brokers**. **Topic** = logical group of **partitions**. **Partition** =
ordered immutable append-only log; ordering guaranteed only **within a partition**.
**Offset** = consumer position in a partition. **Producer** writes; **consumer** reads;
**consumer group** splits partitions among members (more consumers than partitions ⇒ idle
ones; multiple groups ⇒ each gets every message). **Replication factor** 2–3, ideally 3
brokers; one **leader** per partition + **followers** (ISR); leader fails ⇒ follower
elected. Default retention 7 days.

## Partitioning
`partition = murmur2(key) % num_partitions`. Same key → same partition (ordering).
Adding partitions rehashes keys → breaks per-key ordering: recommended fix is a **new
topic** with desired partition count, copy via Kafka Streams/ksqlDB; consistent hashing is
non-standard here. Broker failure does NOT rehash (partition count unchanged) — only
partition→broker mapping changes. See [[patterns/data-partitioning]].

## Delivery & acks
Producer acks: `0` (no wait, data loss) · `1` (leader only, limited loss) · `all`
(leader+ISR, no loss). Three semantics → [[concepts/delivery-semantics]]: at-most-once,
at-least-once (preferred; needs idempotent consumers), exactly-once (Kafka-to-Kafka via
Streams API).

## Why Kafka is fast
Sequential I/O on append-only log (seq writes ~100 MB/s vs random ~100 KB/s) · zero-copy
(`sendfile`: OS cache → NIC buffer via DMA, no app copies) · indexed segment files ·
cheap HDD capacity. **Pull-based** model → consumer controls rate, natural batching,
backpressure; long-polling avoids tight-loop polling. See [[comparisons/push-vs-pull-messaging]].

## Concurrency
Internally non-blocking/async; only application threads block (`producer.send().get()`,
`consumer.poll()`). Multi-threaded consumer: group records by partition, submit to thread
pool, `pause()` partition while in-flight, manual offset commits to preserve per-partition
order + at-least-once. Spring Kafka `concurrency` = N independent consumers (not a pool),
bounded by partition count.

## Hot partition fixes → [[concepts/hot-key-hot-partition]]
Random partitioning (loses ordering) · random salting (complicates aggregation) ·
**compound key** (best) · producer backpressure on lag.

## Production issues
Consumer lag · partition imbalance (skewed keys) · rebalance storms (tune
`max.poll.interval.ms`, use CooperativeStickyAssignor) · duplicates (idempotent consumer)
· message loss (`acks=all` + `min.insync.replicas=2` + `replication.factor≥3`) · broker
disk full · under-replicated partitions · schema-evolution errors (Schema Registry) ·
broker GC pauses · compaction lag. ZooKeeper → **KRaft** (KIP-500): self-managed Raft
metadata quorum, ZK removed in Kafka 4.0.

## Related
[[components/redis]] (Streams as lightweight alternative) ·
[[scenarios/high-volume-notification-dispatcher]] · [[scenarios/webhook-delivery-backoff]]
