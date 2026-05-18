---
title: "Reduce-Side vs. Map-Side Joins"
type: comparison
chapters: [10]
tags: [batch, joins, comparison]
status: solid
updated: 2026-05-16
---

# Reduce-Side vs. Map-Side Joins

The batch join algorithms of [[ch10-batch-processing]]. *(DDIA Ch 10)*

| | Reduce-side (sort-merge) | Map-side (broadcast / partitioned / merge hash) |
|---|---|---|
| Input assumptions | **none** — mappers prepare any data | strong (size / partitioning / sorting) |
| Reducers / sort | yes — partition, sort, shuffle, merge | **no reducers, no sort** |
| Cost | expensive (sort + copy + merge; disk spills) | cheap if assumptions hold |
| Broadcast hash | — | small input fits in memory in every mapper |
| Partitioned hash | — | both inputs partitioned same way → per-partition |
| Map-side merge | — | both partitioned **and** sorted same way |
| Output layout | partitioned/sorted by **join key** | like the **large input** |

## How to choose

Use **reduce-side** when you can't assume anything about the inputs. Use a
**map-side** join when you *can* — and the assumptions usually hold only because
**prior MapReduce jobs already produced the data in that partitioned/sorted form**.
The output layout difference matters for downstream jobs; partitioning metadata lives
in HCatalog / the Hive metastore.

## Related

- [[batch-joins]] · [[mapreduce]] · [[hash-partitioning]] · [[dataflow-engines]]

## Sources

DDIA Ch 10 (Summary; "Map-Side Joins").
