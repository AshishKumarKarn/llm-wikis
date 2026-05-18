---
title: "Hadoop vs. MPP Databases"
type: comparison
chapters: [10]
tags: [batch, mpp, hadoop, comparison, analytics]
status: solid
updated: 2026-05-16
---

# Hadoop vs. MPP Databases

MPP databases had parallel join/grouping algorithms a decade before MapReduce. The
differences are about *philosophy*, not the algorithms. *(DDIA Ch 10)*

| | MPP database | Hadoop (HDFS + MapReduce) |
|---|---|---|
| Model | monolithic, tightly integrated; SQL | general-purpose OS for arbitrary programs |
| Storage | proprietary format; **schema-on-write**, up-front modeling | any bytes; **schema-on-read**, "dump now, model later" |
| Processing | parallel analytic SQL | SQL *and* ML/search/image/arbitrary code |
| Fault handling | abort & retry whole query (queries short) | retry per task; eager disk writes |
| Memory | keep data in memory (hash joins) | sparing memory, spills to disk |

## Diversity of storage & processing

- **Data lake / sushi principle** ("raw data is better"): bringing data together
  fast — even raw — beats up-front ideal modeling; interpretation shifts to the
  consumer ([[schema-on-read-vs-schema-on-write]]). Hadoop often used for ETL.
- One shared cluster runs many processing models on the same files (HBase OLTP +
  Impala MPP, neither using MapReduce) — flexibility a monolithic MPP can't match.

## Designing for frequent faults

MapReduce's task-level recovery + sparing memory makes sense because of **Google's
mixed-use datacenters**: batch jobs run at low priority and can be **preempted** any
time a higher-priority service needs resources (~5%/hr termination — 100×
hardware-failure rate; a 100-task/10-min job → >50% chance one task is killed).
Preemption enables better cluster utilization. Less relevant where preemption is rare
(YARN/Mesos/Kubernetes mostly lack general priority preemption) → motivates
[[dataflow-engines]].

> Convergence: batch engines add declarative/columnar/vectorized features; MPP
> becomes more programmable — "in the end, all just systems for storing & processing
> data."

## Related

- [[mapreduce]] · [[dataflow-engines]] · [[oltp-vs-olap]] ·
  [[schema-on-read-vs-schema-on-write]] · [[data-warehousing]]

## Sources

DDIA Ch 10 ("Comparing Hadoop to Distributed Databases").
