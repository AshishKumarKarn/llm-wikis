---
title: Caching
type: component
tags: [caching, performance, scalability, redis]
sources: [system-design, consistent-hashing-primer]
created: 2026-05-15
updated: 2026-05-15
---

# Caching

A fast-access data layer between the consumer and a slower source of truth. Postgres read
~50 ms vs Redis ~1 ms → ~50× latency win (see [[sources/system-design-study-guide]]).
Use when: high access latency, repeated reads of the same data, expensive
computations/queries, read-heavy load, bandwidth limits.

## Types
App-level (in-memory / object / session) · Database (query / row / full-page) ·
Distributed ([[components/distributed-cache]], CDN, Redis Cluster) · Web (browser / proxy /
server-side) · DNS · Hardware (CPU L1–L3, disk).

## Write/read strategies
| Strategy | Behavior | Trade-off |
|---|---|---|
| Cache-aside (lazy) | App checks cache, on miss loads DB & populates | Simple; first-hit miss, stale risk |
| Write-through | Write cache + DB synchronously | Consistent; slower writes |
| Write-back (write-behind) | Write cache, async flush to DB | Fast writes; data-loss risk on cache crash |
| Read-through | Cache itself loads from source on miss | Simple app; tight coupling |
| Pre-fetching | Proactively load before request | Fewer misses; wasted space if wrong |

## Eviction
LRU (temporal locality; Redis/CPU) · MRU (recent unlikely to reuse) · FIFO (simple) ·
LFU (stable hot set) · TTL (time-bounded freshness) · Random (low overhead fallback).

## Consistency
Write-invalidate (delete key on DB write — common) · write-update (push new value) ·
eventual (scales, tolerates staleness) · strong (write-through/locking; financial).
Root cause of staleness: systems read from cache but write to DB first, leaving a window.

## Placement
Local (fastest, unshared) · distributed (shared, network cost) · edge/CDN (user-near).

## Common problems → [[concepts/hot-key-hot-partition]]
- **Cache stampede / thundering herd:** popular key expires, all requests miss and hit DB
  at once. Fix: request coalescing (single-flight) — most effective; cache warming (TTL
  only); jittered TTLs.
- **Cache consistency:** stale key after DB write. Fix: invalidate-on-write, short TTL,
  accept eventual consistency for feeds/metrics.
- **Hot key:** one key gets disproportionate traffic, overloads one node/shard. Fix:
  replicate key across nodes, in-process fallback cache, rate-limit.
- **Cache penetration:** repeated queries for nonexistent keys bypass cache to DB. Fix:
  cache null/placeholder, **Bloom filter** of valid keys (best practice), rate-limit.
- **Big key (Redis):** value >1 MB or >10k elements → memory skew, blocking ops,
  replication lag. Fix: shard the key, TTL, `UNLINK` not `DEL`, compress.

## Trade-offs
Speed vs freshness · memory vs hit ratio · invalidation complexity vs correctness ·
strategy sophistication (LRU/LFU) vs simplicity (FIFO/random) · distributed (shared,
scalable) vs local (fast, unshared).

## Related
[[components/redis]] · [[components/distributed-cache]] · [[concepts/consistent-hashing]] ·
[[scenarios/rate-limiter]]
