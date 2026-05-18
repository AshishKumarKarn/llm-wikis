---
title: ELK Stack
type: component
tags: [observability, logging, elasticsearch, kibana]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# ELK Stack (Elasticsearch · Logstash · Kibana)

Observability pipeline: collect → process → store → visualize
(see [[sources/system-design-study-guide]]).

## Pipeline
1. **Ingest** — Beats (lightweight shippers) or Logstash collect from apps/servers/cloud.
2. **Process** — Logstash parses, filters, enriches → structured format.
3. **Index & store** — Elasticsearch distributes across nodes, indexes for fast search.
4. **Query** — full-text search, aggregations, anomaly detection.
5. **Visualize** — Kibana dashboards, charts, real-time alerts.

Short form: **Collect (Logstash/Beats) → Store & Search (ES) → Visualize (Kibana).**

## Use cases
Log management (microservices, containers) · monitoring & observability · security
analytics (intrusion detection) · business intelligence.

## Challenges
Elasticsearch clusters need careful tuning at scale · log volume can explode (retention
policies critical) · operational complexity for pipelines and indices · query speed
depends on index design.

## Best practices
Use Beats for lightweight shipping · index lifecycle management (ILM) · RBAC + TLS ·
monitor cluster health via Kibana.

## ELK vs Kafka-based pipeline → [[comparisons/push-vs-pull-messaging]]
ELK (Logstash polling) is pull-based with latency floors. Kafka Streams with fire-and-
forget producer push is better for sub-second drill-downs
(see [[scenarios/multi-tenant-usage-dashboard]]).

## Related
[[scenarios/multi-tenant-usage-dashboard]] · [[comparisons/push-vs-pull-messaging]]
