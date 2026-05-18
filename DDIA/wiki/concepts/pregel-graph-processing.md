---
title: Pregel Graph Processing (BSP)
type: concept
chapters: [10]
tags: [batch, graph, pregel, iterative]
status: developing
updated: 2026-05-16
---

# Pregel Graph Processing (BSP)

## The problem

Offline analysis of an *entire* graph (PageRank, recommendation, ranking) needs
**iterative** algorithms — traverse an edge, propagate info, repeat until a condition
(e.g. transitive closure / convergence). Plain MapReduce does a single pass, so an
external scheduler must rerun it each iteration — inefficient (re-reads the whole
dataset even if little changed). *(DDIA Ch 10)*

> Not to be confused with dataflow engines' operator DAG: there the *dataflow* is a
> graph but the *data* is tuples; in graph processing the **data itself is a graph**.

## The Pregel / BSP model

**Bulk synchronous parallel** (Apache Giraph, Spark GraphX, Flink Gelly). Like
MapReduce's "send a message to a reducer", a **vertex sends messages to other
vertices** (usually along edges). Each iteration calls a function per vertex with its
incoming messages; **the vertex remembers state in memory** across iterations, so it
only processes new messages (no messages → no work). Similar to the **actor model**
([[message-passing-dataflow]]) but vertex state/messages are fault-tolerant & durable,
and communication proceeds in **fixed synchronized rounds**.

## Fault tolerance & parallelism

Exactly-once message delivery despite an unreliable network; recovery by periodic
**checkpointing** of all vertex state (roll back to last checkpoint, or selectively
recover a partition if deterministic + messages logged). Framework partitions by
vertex ID (usually arbitrarily — finding a communication-minimizing partition is
hard) → heavy cross-machine message overhead; intermediate state often bigger than
the graph. **If the graph fits on one machine, single-machine (even single-threaded)
often beats distributed** (GraphChi); distributed only when unavoidable.

## Related concepts

- [[graph-data-models]] (Ch 2) · [[mapreduce]] · [[dataflow-engines]] ·
  [[message-passing-dataflow]] · [[pregel-paper]]

## Sources

DDIA Ch 10 ("Graphs and Iterative Processing").
