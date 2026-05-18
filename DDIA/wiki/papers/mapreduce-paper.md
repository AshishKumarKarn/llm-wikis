---
title: "MapReduce: Simplified Data Processing on Large Clusters (Dean & Ghemawat, 2004)"
type: paper
chapters: [2, 10]
tags: [paper, mapreduce, batch, distributed]
status: developing
updated: 2026-05-16
---

# MapReduce: Simplified Data Processing on Large Clusters

Jeffrey Dean & Sanjay Ghemawat, 6th USENIX OSDI, Dec 2004.

The paper behind [[mapreduce-querying]] (Ch 2's NoSQL query mechanism) and the
foundation of DDIA's [[ch10-batch-processing|batch-processing chapter]]. Google's
original use was building search indexes (5–10 chained jobs). Pure `map`/`reduce`
functions over a distributed filesystem ([[gfs-paper|GFS]]) enable transparent
distributed execution, arbitrary ordering, and **re-run-on-failure** with all-or-
nothing output. Not new vs. 1980s MPP databases — its contribution was scale on
commodity hardware + arbitrary code. Importance now declining (Google moved on);
DDIA keeps it as the clearest teaching abstraction → [[mapreduce]],
[[dataflow-engines]]. *(DDIA Ch 2 & 10)*

## Related

- [[mapreduce]] · [[mapreduce-querying]] · [[dataflow-engines]] · [[gfs-paper]] ·
  [[hadoop-vs-mpp-databases]]
