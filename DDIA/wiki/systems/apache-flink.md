---
title: Apache Flink
type: system
chapters: [10, 11]
tags: [system, stream, dataflow]
status: stub
updated: 2026-05-16
---

# Apache Flink

A [[dataflow-engines|dataflow engine]] (Ch 10) and distributed **stream processor**
(Ch 11). Built around **pipelined execution**. Fault tolerance via periodic rolling
**checkpoints** of operator state to durable storage (HDFS), triggered by barriers —
giving exactly-once within the framework ([[stream-fault-tolerance]]). Used for
stream analytics; Gelly = its Pregel-style graph API. *(DDIA Ch 10 & 11)*

## Related

- [[dataflow-engines]] · [[stream-fault-tolerance]] · [[apache-spark]] ·
  [[apache-samza]]
