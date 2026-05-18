---
title: Dataflow Engines (Spark, Tez, Flink)
type: concept
chapters: [10]
tags: [batch, dataflow, spark, flink, beyond-mapreduce]
status: solid
updated: 2026-05-16
---

# Dataflow Engines (Spark, Tez, Flink)

## The problem with MapReduce: materialization

Every MapReduce job fully **materializes** intermediate state to HDFS files. vs. Unix
pipes (stream incrementally). Downsides: a job can't start until *all* preceding
tasks finish (stragglers slow the workflow); redundant mappers re-read reducer
output; replicating temporary data is overkill. *(DDIA Ch 10)*

## Dataflow engines

Spark, Tez, Flink handle a **whole workflow as one job**. Generalize map/reduce into
arbitrary **operators** connected by: repartition+sort (sort-merge), partition-only
(hash join), or broadcast. Advantages over MapReduce:

- Sort only where needed; no unnecessary map tasks; explicit dataflow → scheduler
  does locality optimizations.
- Intermediate state in memory / local disk (not replicated HDFS); operators start
  as input is ready (pipelined — esp. Flink); reuse JVM processes.
- Same Pig/Hive/Cascading code runs on MapReduce or Tez/Spark via config.

## Fault tolerance without materialized state

If intermediate state is lost, **recompute** it from inputs. Spark tracks data
ancestry via the **RDD** (resilient distributed dataset) abstraction; Flink
checkpoints operator state. Requires **deterministic** operators (else kill & re-run
downstream too) — beware accidental nondeterminism (hash-table iteration order,
random, clock, external data); fix with fixed seeds. Recompute isn't always best
(small intermediate or CPU-heavy → materialize instead).

## Declarative high-level APIs

Relational-style building blocks (join/group/filter/aggregate); **cost-based query
optimizers** (Hive/Spark/Flink) pick join algorithms & order; declarative simple
ops enable columnar reads + **vectorized execution** ([[column-oriented-storage]];
Spark JVM bytecode, Impala LLVM). Retain arbitrary-code flexibility (libraries, ML —
Mahout/MADlib). Batch engines & MPP databases converge.

## Related concepts

- [[mapreduce]] · [[mapreduce-vs-dataflow-engines]] · [[pregel-graph-processing]] ·
  [[hadoop-vs-mpp-databases]] · [[apache-spark]] · [[spark-rdd-paper]]

## Sources

DDIA Ch 10 ("Materialization of Intermediate State", "Dataflow engines",
"High-Level APIs and Languages").
