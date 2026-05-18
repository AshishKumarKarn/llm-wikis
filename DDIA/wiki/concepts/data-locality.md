---
title: Data Locality
type: concept
chapters: [2, 3]
tags: [data-models, performance, storage]
status: developing
updated: 2026-05-16
---

# Data Locality

## Definition

Storing related data physically together so it can be read with fewer disk
seeks/index lookups. A document stored as one continuous string (JSON/XML/BSON) has
locality; data split across tables (Figure 2-1) needs multiple index lookups. *(DDIA
Ch 2)*

## Why it matters

A real performance lever for the [[document-model]] *if* the application often needs
large parts of a document at once (e.g. rendering a whole page).

## How it works / caveats

- The locality benefit applies **only** when you need most of the document together.
  The DB typically loads the **entire** document even for a small field — wasteful
  for large documents. Updates usually **rewrite the whole document** (only
  size-preserving in-place edits are cheap). ⇒ keep documents small, avoid
  size-growing writes; these limits "significantly reduce the set of situations in
  which document databases are useful."
- Locality is **not exclusive to the document model**:
  - **[[google-spanner]]** — interleave child table rows within a parent table.
  - **Oracle** — multi-table index cluster tables.
  - **Column-family** (Bigtable model, used by [[cassandra]]/HBase) — manages
    locality.

## Trade-offs

Whole-record read speed vs. wasteful loads/rewrites of large records on partial
access.

## Related concepts

- [[document-model]] · [[relational-vs-document-model]]
- [[ch03-storage-and-retrieval]] — locality deepened at the storage-engine level

## Sources

DDIA Ch 2 ("Data locality for queries"); more in Ch 3.
