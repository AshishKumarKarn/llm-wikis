---
title: Data Model Layering
type: concept
chapters: [2]
tags: [data-models, abstraction, foundations]
status: developing
updated: 2026-05-16
---

# Data Model Layering

## Definition

Applications are built by **layering one data model on another**, each layer answering
"how is this represented in terms of the next-lower layer?" *(DDIA Ch 2)*

## The layers

1. **App developer** models the real world as objects/data structures + APIs
   (app-specific).
2. Those structures are expressed in a **general-purpose data model**: JSON/XML
   documents, relational tables, or a graph.
3. **Database engineers** represent that model as bytes in memory / on disk / on the
   network, allowing query, search, manipulation.
4. **Hardware engineers** represent bytes as electrical currents, light pulses,
   magnetic fields.

## Why it matters

Each layer **hides the complexity below it** behind a clean model, letting different
groups (DB vendor engineers vs. app developers) collaborate effectively — a concrete
instance of the [[maintainability]] argument that good abstractions manage
complexity. Every data model embodies usage assumptions: some operations easy/fast,
others awkward/slow — so the model choice has a *profound* effect on what the software
above can do. This chapter focuses on **layer 2**; [[ch03-storage-and-retrieval]]
covers layer 3.

## Related concepts

- [[maintainability]] — abstraction as the tool against accidental complexity
- [[relational-model]] · [[document-model]] · [[graph-data-models]]
- [[ch03-storage-and-retrieval]] — the next layer down

## Sources

DDIA Ch 2 (chapter introduction).
