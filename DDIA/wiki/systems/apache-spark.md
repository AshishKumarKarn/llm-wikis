---
title: Apache Spark
type: system
chapters: [10]
tags: [system, batch, dataflow, spark]
status: stub
updated: 2026-05-16
---

# Apache Spark

The best-known **[[dataflow-engines|dataflow engine]]** (with Tez, Flink). Whole
workflow = one job; avoids materializing intermediate state (in memory / local disk);
fault tolerance via **RDD** lineage recomputation ([[spark-rdd-paper]]); high-level
DataFrame API with a cost-based optimizer + vectorized execution (generates JVM
bytecode). GraphX = its Pregel-style [[pregel-graph-processing|graph]] API.

> `status: stub` — may resurface for stream processing (Spark Streaming /
> micro-batching) in [[ch11-stream-processing]].

## Related

- [[dataflow-engines]] · [[mapreduce-vs-dataflow-engines]] · [[spark-rdd-paper]] ·
  [[ch11-stream-processing]]
