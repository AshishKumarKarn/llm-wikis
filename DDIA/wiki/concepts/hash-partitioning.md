---
title: Hash Partitioning
type: concept
chapters: [6]
tags: [partitioning, hash, consistent-hashing]
status: developing
updated: 2026-05-16
---

# Hash Partitioning

## Definition

Apply a hash function to each key; a partition owns a **range of hashes**. A good
hash turns skewed data into a uniform distribution (even similar inputs spread
evenly). Need not be cryptographic (Cassandra/MongoDB: MD5; Voldemort: FNV) but
must be stable across processes (Java `Object.hashCode()` / Ruby `Object#hash` are
*not*). *(DDIA Ch 6)*

## Trade-off

Distributes load evenly but **destroys key ordering** → range queries inefficient
(MongoDB hash mode sends ranges to all partitions; Riak/Couchbase/Voldemort don't
support PK range queries). **Cassandra compromise**: a compound primary key — hash
*only the first column* for the partition, use the rest as a concatenated sort index
→ no range scan on the first column, but efficient range scan on the others given a
fixed first column (elegant for one-to-many, e.g. `(user_id, update_timestamp)`).

## The "consistent hashing" terminology trap

Karger et al.'s **consistent hashing** (randomly chosen boundaries, no central
control — for CDN caches) has *nothing* to do with replica/ACID consistency. It
actually works poorly for databases and is rarely used as originally defined —
DDIA recommends just saying **hash partitioning**. (It does relate to
proportional-to-nodes [[rebalancing-partitions|rebalancing]].)

## Related concepts

- [[key-range-partitioning]] · [[key-range-vs-hash-partitioning]] ·
  [[skewed-workloads-and-hot-spots]] · [[rebalancing-partitions]]

## Sources

DDIA Ch 6 ("Partitioning by Hash of Key").
