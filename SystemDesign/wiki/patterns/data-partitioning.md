---
title: Data Partitioning
type: pattern
tags: [partitioning, sharding, scalability]
sources: [consistent-hashing-primer, system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Data Partitioning

Splitting one logical dataset across multiple nodes so it scales beyond a single machine.
Required for [[concepts/horizontal-scaling|horizontal scaling]] of stateful systems.

## Strategies
- **Hash partitioning** — node = function of `hash(key)`. Even spread, but range scans are
  scattered. Naive `% N` rebalances catastrophically on resize
  (see [[comparisons/modulo-vs-consistent-hashing]]); [[concepts/consistent-hashing]]
  fixes this.
- **Range partitioning** — contiguous key ranges per node. Great for range scans, prone to
  hotspots on skewed keys.
- **Directory / lookup-based** — a lookup service maps key → node. Maximum flexibility,
  but the directory is itself a scaling and availability concern.

## Concerns layered on top
Partitioning answers *placement only*. Replication factor, consistency model, quorum, and
rebalancing are separate decisions stacked on the partitioning scheme
(see [[sources/consistent-hashing-primer]]).

## Applied in
[[components/distributed-cache]] · [[components/kafka]] (partition = hash(key) % N;
repartitioning: create new topic + copy — don't add partitions mid-stream) ·
[[scenarios/distributed-number-inventory]] (sharded by country_code)
