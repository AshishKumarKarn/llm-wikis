---
title: Materialized Views & Data Cubes
type: concept
chapters: [3]
tags: [olap, analytics, derived-data, materialized-view]
status: developing
updated: 2026-05-16
---

# Materialized Views & Data Cubes

## Definition

A **materialized view** is an actual on-disk copy of a query's results (vs. a virtual
view, just a query shortcut expanded on the fly). A **data cube / OLAP cube** is a
grid of aggregates grouped by several dimensions. *(DDIA Ch 3)*

## Why it matters

Warehouse queries repeatedly compute the same aggregates (COUNT/SUM/AVG/…). Caching
them avoids re-crunching raw data. A data cube precomputes, e.g., a date×product grid
of `SUM(net_price)`; "total sales per store yesterday" is then a single dimension
lookup, no million-row scan.

## How it works / trade-offs

- A materialized view is a **denormalized copy** — when source data changes it must
  be updated, making writes more expensive. Hence rare in OLTP, more sensible in
  read-heavy warehouses (benefit is case-dependent). This is a concrete instance of
  [[normalization-and-denormalization]] / derived data.
- **Cube downside:** loss of query flexibility — you can only slice on the chosen
  dimensions (can't ask "share of sales from items > $100" if price isn't a
  dimension). So warehouses keep **raw data** and use cubes only as a targeted speed
  boost.

## Related concepts

- [[normalization-and-denormalization]] — materialized views are derived/denormalized
- [[column-oriented-storage]] · [[data-warehousing]] · [[oltp-vs-olap]]
- [[ch11-stream-processing]] — materialized views as maintained stream outputs
  (Part III derived data)

## Sources

DDIA Ch 3 ("Aggregation: Data Cubes and Materialized Views"). Ref: Gray et al.,
"Data Cube" (2007).
