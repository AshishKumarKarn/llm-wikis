---
title: Apache Samza
type: system
chapters: [11]
tags: [system, stream, kafka]
status: stub
updated: 2026-05-16
---

# Apache Samza

A distributed **stream processor** tightly coupled with [[apache-kafka|Kafka]]. DDIA
Ch 11: supports **materialized-view maintenance** (windows back to the beginning of
time, built on Kafka log compaction), SQL for declarative stream queries, and
**state replication** to a dedicated log-compacted Kafka topic (CDC-like) for
[[stream-fault-tolerance]]. *(DDIA Ch 11)*

## Related

- [[apache-kafka]] · [[stream-fault-tolerance]] · [[change-data-capture]] ·
  [[stream-processing-uses]]
