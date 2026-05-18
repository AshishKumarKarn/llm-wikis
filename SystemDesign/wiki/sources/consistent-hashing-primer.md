---
title: "Source: Primer on Consistent Hashing"
type: source
tags: [hashing, sharding, scalability, caching]
sources: [consistent-hashing-primer]
created: 2026-05-15
updated: 2026-05-15
---

# Source: Primer on Consistent Hashing

**Origin:** `raw/consistent-hashing-primer.md` · seed/demo source · 2026-05-15
**Domain:** data partitioning, distributed caching

## Summary
Modulo hashing (`hash(key) % N`) for placing keys on N servers remaps almost all keys when
N changes, causing a cache stampede onto the database. Consistent hashing maps servers and
keys onto a shared ring; a key belongs to the next server clockwise. A membership change
moves only ~K/N keys instead of nearly all. Virtual nodes (many ring tokens per physical
server) are needed in practice for even load and for weighting heterogeneous hardware.
Used by Dynamo, Cassandra, Riak, memcached/ketama.

## Key takeaways
1. The failure mode of modulo hashing on resize is total remap → mass cache miss.
2. Consistent hashing bounds relocation to ~1/N of keys per membership change.
3. Virtual nodes are not optional in production — naive rings are unbalanced.
4. Consistent hashing solves *placement only*; replication/consistency layer on top.

## Open questions
- How does this interact with replication factor R and quorum reads/writes?
- What's the cost/benefit of token count per node (100 vs 200 vs 1000)?

## Backs these pages
[[concepts/consistent-hashing]] · [[concepts/horizontal-scaling]] ·
[[components/distributed-cache]] · [[patterns/data-partitioning]] ·
[[comparisons/modulo-vs-consistent-hashing]]
