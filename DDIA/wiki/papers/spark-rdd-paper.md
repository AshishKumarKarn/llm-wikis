---
title: "Resilient Distributed Datasets (Zaharia et al., 2012)"
type: paper
chapters: [10]
tags: [paper, batch, spark, dataflow]
status: stub
updated: 2026-05-16
---

# Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing

Matei Zaharia, Mosharaf Chowdhury, Tathagata Das, et al., 9th USENIX NSDI, April
2012.

Defines the **RDD** abstraction behind Apache Spark ([[dataflow-engines]]): tracks
data **ancestry/lineage** so lost in-memory intermediate state can be **recomputed**
instead of materialized — the key to fault tolerance without HDFS round-trips.
*(DDIA Ch 10)*

## Related

- [[dataflow-engines]] · [[apache-spark]] · [[mapreduce-vs-dataflow-engines]]
