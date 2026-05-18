---
title: Normalization & Denormalization
type: concept
chapters: [2]
tags: [data-models, normalization, derived-data]
status: developing
updated: 2026-05-16
---

# Normalization & Denormalization

## Definition

**Normalization** = storing human-meaningful information in exactly one place and
referring to it elsewhere by an ID. **Denormalization** = deliberately duplicating
that information into the records that use it. Rule of thumb: *if you're duplicating
values that could be stored in one place, the schema is not normalized.* *(DDIA Ch 2)*

## Why it matters / what problem it solves

ID vs. text string is a **duplication** question. An ID has no human meaning, so it
*never needs to change* even when the thing it identifies changes. Anything
meaningful to humans (a city name, an industry label) may change — and if duplicated,
every copy must be updated, incurring write overhead and risking **inconsistency**
(some copies updated, others not). Other benefits of normalized IDs: consistent
spelling, disambiguation, localization, better search (the list can encode "Seattle
is in Washington").

## How it works (the tension)

Normalizing requires **many-to-one** relationships (many people → one region/
industry). These do **not** fit the [[document-model]] well: document joins are weak,
so you either emulate joins in app code or denormalize. In the [[relational-model]],
referencing rows by ID is normal because joins are easy.

Denormalization trades read simplicity/speed for **write-time consistency work**:
the application must keep duplicated copies in sync. DDIA explicitly defers the
systematic treatment (caching, denormalization, derived data) to **Part III**.

## Trade-offs

- Normalized: no duplication, safe updates, but needs joins (DB or app-side).
- Denormalized: fast/local reads, but the app must maintain consistency of copies.
- This is the seed of the **derived data** theme — see [[ch10-batch-processing]],
  [[ch11-stream-processing]].

## Related concepts

- [[relational-vs-document-model]] — many-to-one is why interconnected data favors
  relational/graph
- [[data-locality]] · [[graph-data-models]]

## Sources

DDIA Ch 2 ("Many-to-One and Many-to-Many Relationships"). Derived-data follow-up:
Part III.
