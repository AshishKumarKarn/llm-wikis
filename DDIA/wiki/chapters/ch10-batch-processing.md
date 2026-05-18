---
title: "Ch 10 — Batch Processing"
type: chapter
chapters: [10]
tags: [batch, mapreduce, hadoop, dataflow, derived-data]
status: solid
updated: 2026-05-16
---

# Ch 10 — Batch Processing

## One-paragraph thesis

Three system types: **services** (online, response-time-bound), **batch** (offline,
throughput-bound, bounded input → derived output), **stream** (near-real-time, Ch 11).
Batch processing's principles come straight from the **Unix philosophy** (do one
thing well, immutable inputs, composable via a uniform interface). **MapReduce** is
"a distributed Unix": HDFS = the uniform interface, map+sort+reduce = the pipeline,
parallelized over partitions with transparent fault tolerance. The chapter covers
join algorithms, the value of immutable inputs / replaceable outputs ("human fault
tolerance"), how Hadoop differs from MPP databases (schema-on-read "data lake"), and
the move **beyond MapReduce** to dataflow engines (Spark/Tez/Flink) and graph
processing (Pregel). *(DDIA Ch 10; Part III "derived data" — input is **bounded**.)*

## Key ideas

- **[[batch-vs-stream-vs-online]]** — services/batch/stream by latency &
  input-boundedness.
- **[[unix-philosophy]]** — uniform interface, logic↔wiring separation,
  transparency/experimentation; the template for Hadoop.
- **[[mapreduce]]** — mapper→shuffle(partition+sort)→reducer; putting computation
  near the data; workflows chained via HDFS directories + schedulers (Airflow/Oozie).
- **[[distributed-filesystem-hdfs]]** — shared-nothing, NameNode, replication /
  erasure coding; "dump data, decide later".
- **[[batch-joins]]** — reduce-side (sort-merge, secondary sort) vs. map-side
  (broadcast / partitioned / merge hash joins); GROUP BY, sessionization, **hot-key
  skew**.
- **[[batch-workflow-output]]** — build search indexes / read-only KV stores as
  immutable files; **human fault tolerance** (roll back code, rerun).
- **[[hadoop-vs-mpp-databases]]** — diversity of storage/processing; data
  lake/sushi principle; designing for **frequent faults** (Google preemption).
- **[[dataflow-engines]]** — Spark/Tez/Flink: one job, avoid materializing
  intermediate state, RDD lineage / checkpoint recovery, determinism.
- **[[pregel-graph-processing]]** — BSP "think like a vertex"; iterative graph
  algorithms; declarative high-level APIs & query optimizers.

## Concepts introduced

- [[unix-philosophy]] · [[mapreduce]] · [[distributed-filesystem-hdfs]] ·
  [[batch-joins]] · [[batch-workflow-output]] · [[dataflow-engines]] ·
  [[pregel-graph-processing]]

## Comparisons introduced

- [[batch-vs-stream-vs-online]] · [[hadoop-vs-mpp-databases]] ·
  [[reduce-side-vs-map-side-joins]] · [[mapreduce-vs-dataflow-engines]]

## Systems / papers referenced

- Hadoop/HDFS, [[apache-spark]] (RDD), Tez, Flink, Pig/Hive/Cascading/Crunch,
  [[lucene]] (index building), [[voldemort]] (batch-built read-only stores),
  HBase/Impala
- [[mapreduce-paper]] (Dean & Ghemawat 2004 — now expandable), [[gfs-paper]],
  [[spark-rdd-paper]], [[pregel-paper]], PageRank

## Trade-offs & tensions

- MapReduce: robust under frequent task termination (Google preemption ~5%/hr) but
  slow (materializes intermediate state, eager disk writes) vs. dataflow engines:
  faster (pipelined, in-memory) but recompute on failure (need determinism).
- Schema-on-write MPP (quality, query perf) vs. schema-on-read data lake (fast
  collection, flexible, sushi principle).
- Declarative (optimizer picks join algo, columnar, vectorized) vs. arbitrary code
  (libraries, ML, custom).
- Distributed graph processing overhead — single machine often wins if it fits.

## Connections to other chapters

- Unix sort spill-to-disk reuses [[sstables-and-lsm-trees]] mergesort; framework
  fault tolerance ↔ [[mapreduce-querying]] purity (Ch 2); partitioning ↔
  [[hash-partitioning]] (Ch 6); Avro/Parquet inputs ↔ Ch 3/4.
- Materialization ↔ [[materialized-views-and-data-cubes]]; "derived from input" ↔
  [[systems-of-record-and-derived-data]]; expands [[parallel-query-execution]] (Ch 6
  stub).
- Bounded input → unbounded streams → [[ch11-stream-processing]];
  Pregel ↔ actor model ([[message-passing-dataflow]]).

## Open questions / things to revisit

- How does stream processing "speed up" batch (Ch 11)? Lambda/unbundling (Ch 12)?
