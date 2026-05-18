---
title: Secondary, Clustered & Covering Indexes
type: concept
chapters: [3]
tags: [storage, indexes]
status: developing
updated: 2026-05-16
---

# Secondary, Clustered & Covering Indexes

## Definition

A **primary key index** uniquely identifies one row/document/vertex; others reference
it by ID. A **secondary index** is any additional index — crucial for joins (e.g. an
index on `user_id`). Its keys are **not unique**, solved by making the value a list
of row IDs (postings-list style) or appending a row ID to the key. Both B-trees and
log-structured indexes work as secondary indexes. *(DDIA Ch 3)*

## Where the row lives (the value question)

The index value is either the actual row, or a **reference** to it.

- **Heap file** — rows stored in no particular order (append-only or reuse deleted
  slots). Indexes point into it; avoids duplicating data across multiple secondary
  indexes. In-place update is fine if the new value isn't larger; if larger, move +
  update all indexes or leave a forwarding pointer.
- **Clustered index** — store the row data *directly in the index* (no heap hop).
  MySQL InnoDB: primary key is always clustered, secondary indexes refer to the PK.
- **Covering index / index with included columns** — a compromise: store *some*
  columns in the index so a query can be answered from the index alone ("the index
  covers the query").

## Trade-offs

Like any duplication, clustered/covering indexes speed reads but cost storage, add
write overhead, and require extra effort to keep transactionally consistent.

## Related concepts

- [[b-tree]] · [[sstables-and-lsm-trees]] · [[storage-engine-index-tradeoff]]
- [[multi-dimensional-indexes]] — querying several columns at once

## Sources

DDIA Ch 3 ("Other Indexing Structures").
