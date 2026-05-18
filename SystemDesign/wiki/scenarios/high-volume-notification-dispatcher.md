---
title: "Design: High-Volume Notification Dispatcher"
type: scenario
tags: [kafka, notifications, sms, push, rate-limiting, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: High-Volume Notification Dispatcher

**Prompt:** Deliver a single "Broadcast" to 10M users via SMS or Push within 5 minutes.
**Math:** 10M ÷ 300s = **~33,000 messages/second** (see [[sources/system-design-study-guide]], p127).

## Key decisions

### Fan-out via partitioned Kafka
Broadcast API writes one job record + one event. Workers pull from 100+ Kafka partitions.
50 pods × 700 IDs/sec each → 35K/sec fan-out capacity. See [[components/kafka]].

### Pre-segmentation (not real-time lookup)
Don't query 10M rows at broadcast time — too slow. Pre-compute user segments in Redis.
Workers fetch pre-built batches of user IDs. See [[components/redis]].

### Channel routing
Each user's preferred channel (push/SMS) from profile → two sub-queues.
- Push (APNs/FCM): ~25K/sec, 30 pods.
- SMS (Vonage/Twilio): ~8K/sec, capped by gateway.

### SMS gateway rate limiting — token bucket in Redis
Each gateway gets its own token bucket (capacity = burst allowance, refill = gateway RPS
limit). Redis Lua script acquires atomically. Multiple gateways = parallelism + fallback:
- Gateway A → 3,000/sec; B → 2,000/sec; C → 1,500/sec overflow.
- On 429 → re-route to next gateway immediately. On 5xx → exponential backoff with
  jitter (`min(2^n×100ms + random(50ms), 5s)`). Jitter prevents thundering herd on gateway.
- **Redis fallback:** if Redis goes down, local in-memory buckets (approximate). Accept
  slight over-sending vs stalling 10M messages.

### Delivery tracking via async writes
Workers publish delivery events to a Kafka topic → lightweight consumer batch-upserts to
**Cassandra** (optimized for high write throughput). Not direct DB writes (10M hammers).

### Idempotency
Deduplication key = `broadcast_id + user_id`. Check before send — handles worker crash
mid-batch producing duplicates.

### DLQ + jitter
After N retries → DLQ → human alert or retry after cooldown. Spread fan-out with jitter
so APNs/Twilio aren't spiked at T+0.

## Capacity table
| Layer | Target | Design |
|---|---|---|
| Fan-out workers | ~33K IDs/sec | 50 pods × 700/sec |
| Push senders | ~25K push/sec | 30 pods, APNs 1000 req/conn |
| SMS senders | ~8K SMS/sec | 3+ gateways, token buckets |
| Kafka | 100+ partitions | one consumer/partition |
| Cassandra | ~33K writes/sec | batched |

## Related
[[components/kafka]] · [[components/redis]] · [[scenarios/rate-limiter]] ·
[[concepts/delivery-semantics]] · [[patterns/bulkhead]]
