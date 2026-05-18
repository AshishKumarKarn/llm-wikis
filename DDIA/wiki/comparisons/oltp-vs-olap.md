---
title: "OLTP vs. OLAP"
type: comparison
chapters: [3]
tags: [storage, oltp, olap, analytics, comparison]
status: solid
updated: 2026-05-16
---

# OLTP vs. OLAP

A "transaction" = a logical group of reads/writes (no ACID implied — see
[[ch07-transactions]]). The great access-pattern divide of
[[ch03-storage-and-retrieval]]. *(DDIA Ch 3)*

| Property | OLTP (transaction processing) | OLAP (analytics) |
|---|---|---|
| Read pattern | Few records per query, fetched by key | Aggregate over millions of records |
| Write pattern | Random-access, low-latency from user input | Bulk import (ETL) or event stream |
| Used by | End user / customer via web app | Internal analyst, decision support |
| Data represents | Latest state, current point in time | History of events over time |
| Dataset size | GB–TB | TB–PB |
| Bottleneck | **Disk seek time** | **Disk bandwidth** |
| Storage | Row-oriented ([[b-tree]], [[sstables-and-lsm-trees]]) | [[column-oriented-storage]] |

## Why they split

SQL serves both, but ad-hoc analytic scans harm concurrent OLTP transactions, so
since the late 1980s analytics moved to a separate **[[data-warehousing|data
warehouse]]** (a read-only ETL'd copy) optimized for analytic access. Some products
(MS SQL Server, SAP HANA) do both but increasingly as two engines behind one SQL
interface. The "online" in OLAP refers to interactive/explorative queries (Codd et
al. 1993).

## Related

- [[data-warehousing]] · [[star-and-snowflake-schema]] ·
  [[column-oriented-storage]] · [[materialized-views-and-data-cubes]]
- [[ch10-batch-processing]] — SQL-on-Hadoop continues this lineage

## Sources

DDIA Ch 3 ("Transaction Processing or Analytics?").
