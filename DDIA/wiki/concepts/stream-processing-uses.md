---
title: Uses of Stream Processing
type: concept
chapters: [11]
tags: [stream, cep, analytics, materialized-views]
status: developing
updated: 2026-05-16
---

# Uses of Stream Processing

## What you do with a stream

Three options: write to a storage system (keep a DB in sync — streaming equivalent of
batch output), push to users (alerts/dashboards), or **process into derived
streams** (an *operator*/job: read-only input → append-only output, acyclic
pipeline). The crucial difference from batch: **a stream never ends** — no
sort-merge joins, and a years-running job can't just restart from the beginning.
*(DDIA Ch 11)*

## The applications

- **Complex event processing (CEP)** — search for **event patterns** (like a regex
  over a stream): fraud, trading, monitoring. Roles reversed vs. a DB: **queries are
  stored long-term, data flows past them**. Esper, IBM InfoSphere Streams, Samza SQL.
- **Stream analytics** — aggregations/statistics over **windows** (rate, rolling
  average, vs. last week). Often probabilistic algorithms (Bloom filter,
  HyperLogLog, percentile estimators) — an *optimization*, not inherent
  approximation. Storm, Spark Streaming, [[apache-flink|Flink]], [[apache-samza|Samza]],
  Kafka Streams.
- **Maintaining materialized views** — keep caches/indexes/warehouses (and event-
  sourced app state) up to date; needs a window stretching to the **beginning of
  time** (vs. analytics' bounded windows). See [[materialized-views-and-data-cubes]].
- **Search on streams** — store queries, run documents past them (media monitoring,
  real-estate alerts; Elasticsearch percolator); index queries to scale.
- **Messaging/RPC crossover** — actor frameworks differ (concurrency mgmt vs. data
  mgmt; ephemeral 1:1 vs. durable multi-subscriber) but overlap (Storm distributed
  RPC).

## Related concepts

- [[reasoning-about-time-in-streams]] · [[stream-joins]] ·
  [[materialized-views-and-data-cubes]] · [[message-passing-dataflow]] ·
  [[response-time-percentiles]]

## Sources

DDIA Ch 11 ("Processing Streams", "Uses of Stream Processing").
