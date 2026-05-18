---
title: Distributed Cache
type: component
tags: [caching, scalability, sharding]
sources: [consistent-hashing-primer, system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Distributed Cache

A cache whose data is sharded across multiple nodes so total capacity and throughput
exceed what one node can hold. The central design question is **key placement**: given a
key, which node holds it?

## The node-churn problem
If placement uses `hash(key) % N`, then adding or losing a cache node remaps almost every
key. The cache effectively empties at once and every request falls through to the backing
database — a **cache stampede / thundering herd** that can take the database down (see
[[sources/consistent-hashing-primer]]). This makes the cache fragile precisely when you
scale it.

## The fix
Use [[concepts/consistent-hashing|consistent hashing]] with virtual nodes for placement,
so a membership change moves only ~1/N of keys and the rest stay warm. This is why
memcached client libraries ship a ketama (consistent-hashing) ring.

## Out of scope here
Eviction policy, write-through vs write-back, and replication are separate concerns —
consistent hashing only answers *where a key lives*. See [[patterns/data-partitioning]].
