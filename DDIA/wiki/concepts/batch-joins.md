---
title: Batch Joins (Reduce-Side & Map-Side)
type: concept
chapters: [10]
tags: [batch, joins, mapreduce, skew]
status: solid
updated: 2026-05-16
---

# Batch Joins (Reduce-Side & Map-Side)

## Definition

In batch processing, a **join** = resolving *all* occurrences of an association in a
dataset (process all users at once — a full scan, reasonable for analytics, vs. an
index lookup for one record). MapReduce has no indexes. *(DDIA Ch 10)*

## Reduce-side joins (no input assumptions)

- **Sort-merge join**: mappers emit the join key (e.g. user ID) from *both* inputs;
  partitioning + sorting brings same-key records adjacent at one reducer.
  **Secondary sort** orders the user record before activity events so the reducer
  reads it first. Mappers "send messages" to reducers; the key is the address — this
  separates network communication from app logic.
- **GROUP BY** uses the same "bring related data together" pattern (counting,
  summing, top-k, **sessionization**).
- **Skew / hot keys / linchpin objects** (celebrity): one reducer overloaded; subsequent
  jobs wait for the straggler. Mitigations: Pig **skewed join** (sample hot keys →
  random reducer + replicate other input), Crunch sharded join (explicit), Hive map-
  side for hot keys, or two-stage grouping. (Cf. [[skewed-workloads-and-hot-spots]].)

## Map-side joins (require input assumptions; no reducers/sort)

- **Broadcast hash join**: small input fits in memory → each mapper of the large
  input loads it as a hash table (or a read-only on-disk index). Pig "replicated
  join", Hive "MapJoin", Impala.
- **Partitioned hash join**: both inputs partitioned the same way → join per
  partition independently (Hive "bucketed map join").
- **Map-side merge join**: inputs partitioned *and* sorted the same way → merge like
  a reducer would.

Output partitioning differs (reduce-side: by join key; map-side: like the large
input) — matters for downstream jobs; metadata in HCatalog/Hive metastore.

## Related concepts

- [[reduce-side-vs-map-side-joins]] · [[mapreduce]] · [[hash-partitioning]] ·
  [[skewed-workloads-and-hot-spots]] · [[relational-vs-document-model]] (joins)

## Sources

DDIA Ch 10 ("Reduce-Side Joins and Grouping", "Map-Side Joins").
