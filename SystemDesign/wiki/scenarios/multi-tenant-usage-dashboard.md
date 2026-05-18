---
title: "Design: Multi-Tenant API Usage Dashboard"
type: scenario
tags: [analytics, kafka, olap, tiered-storage, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: Multi-Tenant API Usage Dashboard

**Prompt (Vonage):** Real-time dashboard showing API usage, costs, and success rates
across SMS/Voice/Video. Supports sub-second drill-downs
("failed SMS to India, last 10 min") (see [[sources/system-design-study-guide]], p136).

## Key decisions

### Data pipeline — Push (producer side) over Pull
Microservices emit structured events fire-and-forget to Kafka (push) on every API call.
ELK/Logstash-polling is bounded by polling interval → sub-second latency is impossible.
Kafka consumers poll at millisecond intervals (pull internally) — effectively real-time.
Kafka producer is async + bounded queue: if analytics pipeline backs up, it drops events,
never blocks the send path. See [[comparisons/push-vs-pull-messaging]], [[components/elk-stack]].

### Storage — 3-tier OLAP
- **Hot (0–24h):** Apache Druid or ClickHouse → pre-aggregated rollups on (product,
  country, status_code, 1-min bucket) → <200ms for drill-downs.
- **Warm (1–30d):** BigQuery / Redshift → seconds.
- **Cold (30d+):** S3 Parquet with lifecycle rules (after 90d → Glacier, ~70% cost cut);
  Athena for queries with partition pruning `year=/month=/day=/product=/region=`.

### Transition mechanics (candidates miss this)
- Hot→Warm: Druid TTL + compaction job re-aggregates 1-min → 1-hour rollups before
  BigQuery (~60× smaller). Keep 2–4h overlap to absorb late-arriving events.
- Warm→Cold: nightly BigQuery→S3 Parquet export, then S3 lifecycle rules.
- **Query router:** dispatches by time range; cross-tier queries fan out + merge in memory.
  Dashboard never knows which tier served the data.

### Isolation from the message path (critical constraint)
Three layers: (1) Kafka producer is fire-and-forget, drops events not messages;
(2) dashboard reads from Druid replicas, never OLTP primary; (3) dashboard API has
30-second Redis TTL cache on aggregate metrics.

## Related
[[components/kafka]] · [[components/elk-stack]] · [[components/redis]] ·
[[components/object-storage-s3]] · [[comparisons/push-vs-pull-messaging]]
