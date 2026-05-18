---
title: Column-Oriented Storage
type: concept
chapters: [3]
tags: [olap, analytics, storage, compression, columnar]
status: solid
updated: 2026-05-16
---

# Column-Oriented Storage

## Definition

Instead of storing all values of one **row** together (row-oriented, typical OLTP /
document storage), store all values of each **column** together — each column in a
separate file, all column files in the **same row order** so the kth entry of every
file is the same row. *(DDIA Ch 3)*

## Why it matters

Analytic ([[oltp-vs-olap|OLAP]]) queries scan millions of rows but touch only ~4–5 of
a 100+-column fact table. Row stores must load every full row from disk then filter;
a column store reads **only the needed columns** — huge I/O savings. Applies to
non-relational too (Parquet = columnar document format, from Google Dremel).

## Mechanisms

- **Column compression** — column values are repetitive. **Bitmap encoding**: a
  column with n distinct values → n bitmaps (one bit/row). Sparse bitmaps are
  **run-length encoded** → remarkably compact. `IN`/`AND` filters become fast bitwise
  OR/AND over bitmaps.
- **Vectorized processing** — operate on compressed column chunks that fit in CPU L1
  cache in tight loops (no per-record function calls); helps memory→CPU bandwidth,
  branch prediction, SIMD. The other bottleneck besides disk bandwidth.
- **Sort order** — sort the whole table by chosen columns (rows kept aligned across
  files). First sort key gets long runs → great run-length compression; aids range
  scans. **C-Store/Vertica** ([[vertica-cstore]]) store the *same data in several
  sort orders* (replicas needed anyway) to fit different query patterns.
- **Writes** — in-place update is impossible on compressed sorted columns; reuse the
  **LSM trick** (memtable → bulk merge into column files; queries merge memory +
  disk). See [[sstables-and-lsm-trees]].

> Note: Cassandra/HBase "column families" (from [[bigtable-paper]]) are **not**
> column-oriented — they store a row's columns together, no column compression;
> still mostly row-oriented.

## Trade-offs

Fast analytic reads & compression vs. harder/more expensive writes (read-mostly
warehouses tolerate this).

## Related concepts

- [[oltp-vs-olap]] · [[star-and-snowflake-schema]] · [[data-warehousing]]
- [[materialized-views-and-data-cubes]] · [[sstables-and-lsm-trees]] (write path)

## Sources

DDIA Ch 3 ("Column-Oriented Storage"). Refs: C-Store (VLDB 2005); Vertica
([[vertica-cstore]]); Abadi et al. "Modern Column-Oriented Database Systems" (2013).
