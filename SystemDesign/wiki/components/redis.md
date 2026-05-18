---
title: Redis
type: component
tags: [redis, caching, distributed-lock, rate-limiting, data-structures]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Redis

In-memory, single-threaded "data structure store" written in C — fast and easy to reason
about, the system-design interview Swiss-army knife (see [[sources/system-design-study-guide]]).
~O(100k) writes/sec, microsecond reads. Trade-off: durability is weak (AOF mitigates;
AWS MemoryDB trades speed for disk durability).

## Data structures
Strings, hashes, lists, sets, sorted sets (priority queues), bloom filters, geospatial
indexes, time series; plus Pub/Sub and Streams.

## Infrastructure
Single node · HA replica · **cluster**: clients cache hash-slot→node map; slot moves
return `MOVED` and client refreshes; gossip protocol between nodes. All data for a request
should live on one node — **key design is how you scale Redis**.

## Capabilities (interview patterns)
- **Cache:** key→value with TTL; eviction by TTL. See [[components/caching]].
- **Distributed lock:** `INCR`+TTL simple lock; Redlock + fencing tokens for rigor.
  Prefer DB consistency if available before adding a lock.
- **Leaderboard:** sorted sets, log-time ranked queries (`ZADD`/`ZREMRANGEBYRANK`).
- **Rate limiter:** fixed window via `INCR`+`EXPIRE`; sliding window via sorted set of
  timestamps in a Lua script (atomic). See [[scenarios/rate-limiter]].
- **Proximity search:** `GEOADD`/`GEOSEARCH`, geohash + radius refine, O(N+log M).
- **Event sourcing / work queue:** Streams (`XADD`) + consumer groups
  (`XREADGROUP`/`XCLAIM`) — Kafka-like, failed worker's messages re-claimable.
- **Pub/Sub:** sharded (`SPUBLISH`/`SSUBSCRIBE`), at-most-once, not durable; one
  connection per node (millions of channels OK). Roll-your-own pub/sub adds hops +
  heartbeat overhead — use native.

## Shortcoming
**Hot key** under skewed load overwhelms one node. Fixes: client in-memory cache,
duplicate key across nodes + randomize, read replicas scaled with load. See
[[concepts/hot-key-hot-partition]].

## Related
[[components/kafka]] · [[components/caching]] · [[scenarios/distributed-number-inventory]]
(Redis `SET NX PX` lock) · [[scenarios/follow-me-voice-routing]] (Redis call state)
