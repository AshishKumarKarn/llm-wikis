---
title: C-Store / Vertica
type: system
chapters: [3]
tags: [system, olap, column-store, data-warehouse]
status: stub
updated: 2026-05-16
---

# C-Store / Vertica

C-Store (research, VLDB 2005) and its commercial successor **Vertica** — canonical
[[column-oriented-storage]] data warehouses. Introduced storing the **same data in
several different sort orders** (replicas are needed anyway, so sort each copy for a
different query pattern) and the **LSM-style write path** (memtable → bulk merge into
column files).

> `status: stub` — Ch 3 only.

## Related

- [[column-oriented-storage]] · [[data-warehousing]] · [[sstables-and-lsm-trees]]
