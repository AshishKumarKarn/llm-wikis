---
title: MapReduce
type: concept
chapters: [10]
tags: [batch, mapreduce, hadoop, distributed]
status: solid
updated: 2026-05-16
---

# MapReduce

## Definition

"A distributed Unix": a blunt but effective batch framework over a distributed
filesystem ([[distributed-filesystem-hdfs|HDFS]]). One job ≈ one Unix process — reads
inputs, produces outputs, **no side effects**, output written once sequentially.
*(DDIA Ch 10; the query-side view is [[mapreduce-querying]], Ch 2.)*

## Job execution (4 steps)

1. Read input files, break into **records** (input format parser).
2. **Mapper** — called once per record, extracts key-value pairs (any number,
   stateless per record).
3. **Sort** — implicit; mapper output always sorted before reduce.
4. **Reducer** — called per key with an iterator over all its values; produces
   output records.

A second sort = a second MapReduce job (workflows chained via HDFS directories +
schedulers: Oozie/Airflow/Luigi; or higher-level Pig/Hive/Cascading/Crunch).

## Distributed execution

- Parallelized by **partitioning** ([[hash-partitioning]]): one map task per input
  file block; reducer count is configurable; key hash → reducer.
- **Putting computation near the data**: scheduler runs the mapper on a machine
  holding a replica of the input block (saves network, increases locality); copies
  the code to the machine.
- **The shuffle**: each mapper partitions output by reducer, sorts locally (SSTable-
  like — [[sstables-and-lsm-trees]]); reducers fetch & merge-sort their partition.
- **Transparent fault tolerance**: framework retries failed tasks; output of failed
  tasks discarded; all-or-nothing job output (only safe because inputs are immutable
  — [[mapreduce-querying|pure functions]]).

## Why study it

Importance now declining, but a clear simple abstraction. Hard to use raw (implement
joins from scratch — [[batch-joins]]) → higher-level APIs & [[dataflow-engines]].
Designed to tolerate **frequent task termination** — see [[hadoop-vs-mpp-databases]].

## Related concepts

- [[mapreduce-querying]] · [[distributed-filesystem-hdfs]] · [[batch-joins]] ·
  [[unix-philosophy]] · [[dataflow-engines]] · [[mapreduce-paper]]

## Sources

DDIA Ch 10 ("MapReduce and Distributed Filesystems").
