---
title: "Source: System Design Study Guide"
type: source
tags: [caching, kafka, kubernetes, redis, aws, load-balancing, scenarios, interview]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Source: System Design Study Guide

**Origin:** `raw/System Design.pdf` · 150 pages · personal study notes (OneNote export),
Jan–May 2026 · interview prep (EM / Solutions Architect, Vonage-flavored)

## Summary
A broad, dense system-design interview guide. Covers caching end-to-end, consistent
hashing, load balancing, a deep Kafka section, Kubernetes, Redis as a multi-tool,
the ELK stack, the AWS data/compute family (S3, Aurora, Lambda, EC2), the N+1 query
anti-pattern, latency percentiles, streaming protocols, and six worked design-problem
walkthroughs with interviewer follow-ups.

## Section index → wiki pages
- Caching (p1–10) → [[components/caching]], [[concepts/hot-key-hot-partition]]
- Consistent Hashing (p11–17) → [[concepts/consistent-hashing]], [[comparisons/modulo-vs-consistent-hashing]]
- Load Balancer (p18–21) → [[components/load-balancer]]
- Kafka (p22–49) → [[components/kafka]], [[concepts/delivery-semantics]], [[comparisons/push-vs-pull-messaging]]
- Kubernetes (p50–76) → [[components/kubernetes]]
- Redis (p77–84) → [[components/redis]]
- ELK (p85–86) → [[components/elk-stack]]
- S3 (p87–90) → [[components/object-storage-s3]]
- Aurora (p91–101) → [[components/aws-aurora]], [[comparisons/aurora-vs-rds]]
- Lambda (p102–108) → [[components/aws-lambda]]
- EC2 / ASG (p109–114) → [[components/aws-ec2]]
- N+1 query (p116–119) → [[patterns/n-plus-1-query]]
- Latency P90/P95/P99 (p120) → [[concepts/latency-percentiles]]
- Streaming protocols (p121–125) → [[concepts/streaming-protocols]]
- Design problems (p126–148) → [[scenarios/rate-limiter]], [[scenarios/high-volume-notification-dispatcher]], [[scenarios/distributed-number-inventory]], [[scenarios/multi-tenant-usage-dashboard]], [[scenarios/follow-me-voice-routing]], [[scenarios/webhook-delivery-backoff]]

## Key takeaways
1. Recurring interview theme: a scheme is judged by its behavior **under change/failure**
   (cache stampede on expiry, hot keys, modulo rehash, Kafka rebalance, Aurora failover).
2. Redis is positioned as the system-design "Swiss-army knife" — cache, lock, leaderboard,
   rate limiter, geo, streams, pub/sub.
3. The design problems converge on a small toolkit: partitioned Kafka, Redis token
   buckets, sagas, DLQs, idempotency keys, backpressure, jitter.
4. Vonage-specific framing throughout (SMS gateways, webhooks, voice routing, numbers).

## Open questions / gaps
- "Dealing With Contention" and "Managing Long Running Tasks" (p149–150) are headers
  with no body in the export — candidates for a future source or web fill.
- Latency-percentiles section is just a link; page is written from general knowledge,
  flagged for a real source.

## Backs these pages
See section index above — this source backs ~30 wiki pages.
