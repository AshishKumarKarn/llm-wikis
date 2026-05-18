---
title: "MapReduce vs. Dataflow Engines"
type: comparison
chapters: [10]
tags: [batch, mapreduce, dataflow, comparison]
status: solid
updated: 2026-05-16
---

# MapReduce vs. Dataflow Engines

*(DDIA Ch 10; Unix analogy: MapReduce ≈ temp files between commands, dataflow ≈
pipes.)*

| | MapReduce | Dataflow engines (Spark/Tez/Flink) |
|---|---|---|
| Job unit | many independent jobs (chained via HDFS dirs) | whole workflow = one job |
| Operators | rigid alternating map/reduce | arbitrary operators, flexible wiring |
| Intermediate state | **fully materialized** to replicated HDFS | in memory / local disk, pipelined |
| Sorting | always between map & reduce | only where required |
| Start | job waits for all preceding tasks | operators start as input is ready |
| JVM | new JVM per task | reuse processes |
| Fault tolerance | re-read durable input (easy) | **recompute** via RDD lineage / checkpoints (needs determinism) |
| Speed | robust under preemption, but slow | often orders of magnitude faster |

## The trade-off

MapReduce's materialization buys **easy fault tolerance & robustness under frequent
task termination** ([[hadoop-vs-mpp-databases]]) at the cost of speed. Dataflow
engines are much faster but must **recompute lost state** (so operators must be
deterministic, else cascade-kill downstream). Recompute isn't always cheaper than
materializing (small or CPU-heavy intermediate). Inputs/final outputs are still
usually immutable HDFS files in both.

## Related

- [[mapreduce]] · [[dataflow-engines]] · [[pregel-graph-processing]] ·
  [[reduce-side-vs-map-side-joins]]

## Sources

DDIA Ch 10 ("Beyond MapReduce", "Discussion of materialization").
