---
title: Star & Snowflake Schemas
type: concept
chapters: [3]
tags: [olap, analytics, data-modeling, dimensional]
status: developing
updated: 2026-05-16
---

# Star & Snowflake Schemas

## Definition

The formulaic analytic data model (a.k.a. **dimensional modeling**). A central
**fact table** — each row an event (a sale, a page view, a click) — surrounded by
**dimension tables** (the who/what/where/when/how/why), referenced by foreign keys.
The radiating shape gives the **star schema** its name. *(DDIA Ch 3)*

## How it works

- Fact tables capture individual events for maximum analytic flexibility → they get
  **huge** (tens of PB at Apple/Walmart/eBay). Columns are either numeric
  **attributes** (price, cost) or **foreign keys** to dimensions. Even date/time is
  often a dimension (so queries can know holidays).
- **Snowflake schema**: dimensions further broken into subdimensions (e.g. separate
  brand/category tables). More normalized than star, but star is usually preferred —
  simpler for analysts.
- Warehouse tables are **very wide**: fact tables often 100+ columns; dimension
  tables include all potentially-relevant metadata.

## Why it matters

The width + "queries touch only 4–5 columns" combination is exactly what motivates
**[[column-oriented-storage]]**.

## Related concepts

- [[oltp-vs-olap]] · [[data-warehousing]] · [[column-oriented-storage]]
- [[normalization-and-denormalization]] — star (denormalized) vs. snowflake

## Sources

DDIA Ch 3 ("Stars and Snowflakes"). Ref: Kimball & Ross, *The Data Warehouse
Toolkit*.
