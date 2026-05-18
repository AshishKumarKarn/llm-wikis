---
title: Multi-Column & Multi-Dimensional Indexes
type: concept
chapters: [3]
tags: [storage, indexes, geospatial]
status: developing
updated: 2026-05-16
---

# Multi-Column & Multi-Dimensional Indexes

## Definition

Single-key indexes can't query several columns simultaneously. *(DDIA Ch 3)*

- **Concatenated index** — combine fields into one key by appending columns in a
  declared order (like a phone book `(lastname, firstname) → number`). Finds by
  lastname or lastname+firstname, but **useless for firstname alone**.
- **Multi-dimensional index** — query several columns at once; important for
  **geospatial** data. A 2D range query (`lat BETWEEN … AND lng BETWEEN …`) can't be
  answered efficiently by a B-tree/LSM (one dimension at a time). Options:
  space-filling curve → regular B-tree; or specialized **R-trees** (PostGIS on
  PostgreSQL GiST).

## Why it matters / generalization

Not just geography: a 3D `(red, green, blue)` index for color search, or 2D
`(date, temperature)` to find 2013 observations at 25–30 °C without scanning all of
one dimension then filtering (used by HyperDex).

## Related concepts

- [[secondary-indexes]] · [[b-tree]] · [[full-text-and-fuzzy-indexes]]

## Sources

DDIA Ch 3 ("Multi-column indexes").
