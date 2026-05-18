---
title: Consistent Hashing
type: concept
tags: [hashing, sharding, scalability]
sources: [consistent-hashing-primer, system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Consistent Hashing

A key-placement scheme that maps both nodes and keys onto a shared circular keyspace (the
**hash ring**). A key is owned by the first node found walking clockwise from the key's
hash position. Its defining property: adding or removing a node relocates only **~K/N
keys** (K total keys, N nodes), not nearly all of them as with [[comparisons/modulo-vs-consistent-hashing|modulo hashing]].

## Why it matters
It is the enabling technique for [[concepts/horizontal-scaling|horizontally scaling]]
stateful systems — [[components/distributed-cache|distributed caches]] and sharded
databases — without a full data reshuffle on every membership change
(see [[sources/consistent-hashing-primer]]).

## Virtual nodes
A naive ring assigns one position per physical node, which produces uneven arcs and
unbalanced load. **Virtual nodes** place each physical node at many ring positions
(~100–200 tokens). This evens out distribution and lets a more powerful node carry
proportionally more tokens (capacity weighting). In practice virtual nodes are mandatory.

## Scope and limits
Consistent hashing solves **placement only**. Replication, consistency, and quorum are
separate concerns layered on top — e.g. replicate a key to the next R nodes clockwise.
See [[patterns/data-partitioning]] for where this sits among partitioning strategies.

## Used by
Amazon Dynamo, Apache Cassandra, Riak, memcached client libraries (ketama).
