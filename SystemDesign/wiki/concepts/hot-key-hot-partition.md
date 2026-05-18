---
title: Hot Key / Hot Partition
type: concept
tags: [scalability, caching, kafka, redis, distributed-systems]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Hot Key / Hot Partition

A recurring pattern where one key or partition receives traffic so disproportionate to
others that it becomes a bottleneck — even when the system as a whole is not saturated
(see [[sources/system-design-study-guide]]).

## Where it appears
- **Caches** ([[components/caching]], [[components/redis]]): one Redis node overwhelmed
  (e.g. Taylor Swift's user key). Fix: replicate key across nodes + randomize, in-process
  fallback cache, read replicas, rate-limit.
- **Kafka** ([[components/kafka]]): one partition gets 80% of traffic due to skewed key
  (e.g. a single high-traffic customer ID). Fix: compound key, random salting, custom
  partitioner, or null key (random partition, lose ordering).
- **Databases**: one shard gets disproportionate writes due to sequential keys or hot
  accounts. Fix: shard by a higher-cardinality dimension, add jitter.

## Compound-key strategy (best for Kafka)
Combine the hot field with a secondary field (`customerId + region`). Most balanced
option — maintains locality without sacrificing even distribution too much.

## General principle
A hot-key problem is essentially a **load imbalance** caused by non-uniform data access
patterns. The fix is always some form of spreading: replication, salting, or re-sharding.
The cost is usually consistency, complexity, or ordering guarantees.

## Related
[[components/caching]] · [[components/redis]] · [[components/kafka]] ·
[[concepts/consistent-hashing]] (spreads load via virtual nodes)
