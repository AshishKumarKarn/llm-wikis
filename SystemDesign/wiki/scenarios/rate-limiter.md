---
title: "Design: Rate Limiter"
type: scenario
tags: [rate-limiting, redis, distributed-systems, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: Rate Limiter

**Prompt:** Global rate-limiting service handling millions of API requests/second across
multiple regions (see [[sources/system-design-study-guide]], p126).

## Key challenge
Ensuring consistency across regions without adding significant latency to every API call.

## Core algorithms
| Algorithm | Mechanism | Trade-off |
|---|---|---|
| Fixed window | `INCR`+`EXPIRE` in Redis | Simple; burst at window boundary |
| Sliding window | Sorted set of timestamps + Lua (`ZADD`/`ZREMRANGEBYCOUNT`) | Accurate; higher memory |
| Token bucket | Bucket capacity + refill rate; decrement on each request | Handles bursts gracefully; the right answer for SMS gateways |
| Leaky bucket | Queue drains at fixed rate | Smoothest output; adds latency |

## Architecture for global scale
- **Centralized Redis** (per region): low-latency within region; cross-region consistency
  hard. Accept eventual consistency between regions unless required.
- **Local in-memory caches** with periodic sync: reduces Redis roundtrip; slight over-
  admission acceptable.
- Thundering herd: if many distributed clients for one API key suddenly appear, the Redis
  bucket absorbs the burst via token bucket capacity.

## Token bucket in Redis (see [[scenarios/high-volume-notification-dispatcher]])
```lua
-- Atomic Lua script on KEYS[1]=gateway:tokens
-- ARGV: now, refill_rate, capacity
```
Lua script ensures atomicity (no race between check-and-decrement across pods).

## Related
[[components/redis]] · [[scenarios/high-volume-notification-dispatcher]] ·
[[components/caching]]
