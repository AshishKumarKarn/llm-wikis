---
title: "Design: Webhook Delivery with Exponential Backoff"
type: scenario
tags: [webhooks, retry, backoff, bulkhead, idempotency, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: Webhook Delivery with Exponential Backoff

**Prompt (Vonage):** Deliver "Delivery Receipt" webhooks to customer URLs reliably even
when their server is down, without overwhelming small customers
(see [[sources/system-design-study-guide]], p146).

## Four interlocking concerns

### 1. Reliability — exponential backoff with full jitter
`delay = random(0, min(cap, base × 2^n))`. Full jitter desynchronizes retrying jobs:
without it all failed jobs at T+0 retry in lockstep at T+30s → thundering herd on a
just-recovered server. **Re-enqueue retries** (don't sleep): sleeping worker blocks a
thread; re-enqueued message is free to process others.

### 2. Isolation — per-customer queue shards (Bulkhead)
Hash on `customer_id` → dedicated sub-queue per customer. Concurrency cap per customer:
- Standard: 50 in-flight.
- Enterprise: 500 (negotiated).
- Trial: 5 (isolated so they can't affect standard tier).
One slow customer (30s response) fills their own bulkhead, not the shared pool.
See [[patterns/bulkhead]].

### 3. Security — HMAC payload signing
```
Vonage-Signature: t=1712345678,v1=HMAC-SHA256(customer_secret, timestamp + "." + body)
```
Timestamp prevents replay attacks (reject if `|now - t| > 300s`). Customer validates
by computing same HMAC. Signing happens at ingest (once per event), not per retry.

### 4. DLQ + observability
After N attempts (typically 5) → DLQ → alert + customer-facing event. Operators replay
DLQ on server recovery. DLQ patterns surface chronic slow customers → proactive outreach.

## Key trade-offs
- Push vs Pull: push is default; pull (customer polls) breaks real-time contract.
- Queue tech: SQS with per-customer visibility timeouts; Kafka with per-customer
  partitions better at Vonage scale.
- Idempotency: send stable `webhook_id` so customers can deduplicate redeliveries.

## Related
[[components/kafka]] · [[patterns/bulkhead]] · [[concepts/delivery-semantics]] ·
[[scenarios/high-volume-notification-dispatcher]]
