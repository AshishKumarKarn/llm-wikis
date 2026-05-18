---
title: Relational Model
type: concept
chapters: [2]
tags: [data-models, relational, sql, query-optimizer]
status: solid
updated: 2026-05-16
---

# Relational Model

## Definition

Data organized into **relations** (SQL: tables), each an *unordered* collection of
**tuples** (rows). Proposed by Edgar Codd in 1970; SQL/RDBMS became dominant by the
mid-1980s and has stayed dominant ~25–30 years across far broader use cases than its
original business-data-processing roots. *(DDIA Ch 2)*

## Why it matters

It won the 1970s "great debate" against the hierarchical (IMS) and network (CODASYL)
models, and its core insight still structures how we think about data and queries.

## How it works (the key insight)

- **Lay all data in the open**: a table is just a collection of rows; no labyrinthine
  nested structures, no hand-followed **access paths**. Read any rows matching an
  arbitrary condition; insert without worrying about foreign-key wiring.
- **The query optimizer makes access-path choices automatically** (which indexes,
  join methods, execution order). This is *the* differentiator vs. CODASYL, where the
  programmer hand-coded access paths and any data-model change forced rewriting
  query code.
- **Build the optimizer once, all apps benefit.** Hand-coding a path for one query is
  easier than writing a general optimizer, but the general-purpose solution wins long
  term. Declare a new index and existing queries use it without modification → easy
  to add features. See [[declarative-vs-imperative-queries]].
- Many-to-one / many-to-many handled via **foreign keys** resolved by joins at query
  time (vs. CODASYL doing the join at insert time).

## Trade-offs

- vs. [[document-model]]: better joins & many-to-many, but
  [[object-relational-impedance-mismatch]] and weaker whole-record
  [[data-locality]]. Full comparison: [[relational-vs-document-model]].
- Query optimizers are "complicated beasts" — decades of R&D — but the cost is paid
  once by the vendor.

## Related concepts

- [[document-model]] · [[graph-data-models]] · [[normalization-and-denormalization]]
- [[declarative-vs-imperative-queries]] — SQL as the declarative exemplar
- [[ch03-storage-and-retrieval]] — how relations are physically stored/indexed

## Sources

DDIA Ch 2. [[codd-relational-model]] (CACM 1970); Bachman "The Programmer as
Navigator" (1973, the CODASYL view).
