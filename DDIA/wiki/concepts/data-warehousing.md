---
title: Data Warehousing & ETL
type: concept
chapters: [3]
tags: [olap, analytics, etl, data-warehouse]
status: developing
updated: 2026-05-16
---

# Data Warehousing & ETL

## Definition

A **data warehouse** is a separate, read-only database holding a copy of data from
all of a company's OLTP systems, that analysts can query freely without affecting
OLTP operations. *(DDIA Ch 3)*

## Why it matters

OLTP databases are business-critical (high availability, low latency); DBAs resist
ad-hoc analytic queries that scan large data and harm concurrent transactions. The
warehouse isolates analytics and can be **optimized for analytic access patterns** —
the OLTP-friendly indexes of the first half of Ch 3 are poor for analytics.

## How it works (ETL)

**Extract–Transform–Load**: extract from OLTP DBs (periodic dump or continuous
stream), transform into an analysis-friendly schema, clean, load into the warehouse.
Ubiquitous in large enterprises, rare in small ones (fewer OLTP systems, smaller
data). Warehouse data model is usually relational (SQL fits analytics; drill-down,
slice-and-dice tools).

## Ecosystem

Commercial: Teradata, Vertica, SAP HANA, ParAccel (Amazon RedShift = hosted
ParAccel). Open-source **SQL-on-Hadoop**: Hive, Spark SQL, Impala, Presto, Tajo,
Drill — several from Google's **Dremel** lineage (→ [[ch10-batch-processing]]).

## Related concepts

- [[oltp-vs-olap]] · [[star-and-snowflake-schema]] · [[column-oriented-storage]]
- [[ch10-batch-processing]] — the batch/derived-data continuation

## Sources

DDIA Ch 3 ("Data Warehousing"). Ref: Chaudhuri & Dayal, "Overview of Data
Warehousing and OLAP" (1997).
