---
title: Parallel Query Execution (MPP)
type: concept
chapters: [6, 10]
tags: [partitioning, mpp, analytics, query]
status: developing
updated: 2026-05-16
---

# Parallel Query Execution (MPP)

Most NoSQL distributed stores support only single-key reads/writes (plus
scatter/gather on document-partitioned secondary indexes). **Massively parallel
processing (MPP)** relational/analytic products go further: the query optimizer
breaks a complex query (joins, filtering, grouping, aggregation) into execution
stages + partitions, many run in parallel across cluster nodes — especially valuable
for large scans. *(DDIA Ch 6)*

## Ch 10: the parallel join/grouping algorithms

DDIA Ch 10 details the actual techniques (already in MPP databases a decade before
MapReduce — Gamma, Teradata, Tandem): partition + sort + merge for sort-merge joins;
broadcast & partitioned hash joins ([[batch-joins]],
[[reduce-side-vs-map-side-joins]]); cost-based optimizers reorder joins to minimize
intermediate state ([[dataflow-engines]]). MPP focuses on parallel analytic SQL;
Hadoop generalizes to arbitrary code ([[hadoop-vs-mpp-databases]]).

## Related

- [[batch-joins]] · [[dataflow-engines]] · [[hadoop-vs-mpp-databases]] ·
  [[local-vs-global-secondary-indexes]] (scatter/gather) · [[oltp-vs-olap]] ·
  [[data-warehousing]]

## Sources

DDIA Ch 6 ("Parallel Query Execution"); Ch 10 ("Comparing Hadoop to Distributed
Databases").
