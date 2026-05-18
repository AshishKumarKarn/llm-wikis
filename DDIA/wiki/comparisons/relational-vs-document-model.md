---
title: "Relational vs. Document Model"
type: comparison
chapters: [2]
tags: [data-models, relational, document, comparison]
status: solid
updated: 2026-05-16
---

# Relational vs. Document Model

The central comparison of [[ch02-data-models-and-query-languages]]. Concentrates on
the *data model* only (fault-tolerance → [[ch05-replication]], concurrency →
[[ch07-transactions]]).

## The core question

Which model leads to simpler application code? **It depends on the relationship
structure of the data**, not on a universal winner.

| Dimension | [[document-model]] | [[relational-model]] |
|---|---|---|
| One-to-many (tree) data | Natural — store nested, load whole tree in one read | Requires "shredding" into multiple tables → cumbersome |
| Many-to-one / many-to-many | Weak; joins often unsupported, emulate in app code | Strong; joins easy, query optimizer handles them |
| Schema | [[schema-on-read-vs-schema-on-write\|schema-on-read]] (flexible, heterogeneous) | schema-on-write (enforced, documented) |
| [[data-locality]] | Good for whole-document access; wasteful for partial / large docs | Multiple index lookups, but no whole-doc penalty |
| Closeness to app objects | Often closer (less [[object-relational-impedance-mismatch]]) | Needs ORM translation layer |
| Referencing a nested item | Awkward ("2nd item in positions list" — like an access path) | Direct via keys |

## The decisive argument

> "It's not possible to say in general which data model leads to simpler application
> code; it depends on the kinds of relationships that exist between data items. For
> highly interconnected data, the document model is awkward, the relational model is
> acceptable, and graph models are the most natural." *(DDIA Ch 2)*

**Key dynamic:** even if v1 fits a join-free document model, *data tends to become
more interconnected as features are added* (résumé → organizations-as-entities →
recommendations referencing author profiles). Denormalizing to avoid joins pushes
consistency-maintenance work into the application — revisited as derived data in
Part III.

## Convergence

Relational DBs (PostgreSQL ≥9.3, MySQL ≥5.7, DB2 ≥10.5) added JSON/XML with
in-document indexing/querying; document DBs (RethinkDB joins; MongoDB driver-side
reference resolution) added relational features. Codd's original 1970 model already
allowed "nonsimple domains" (nested relations). DDIA's verdict: a relational/document
**hybrid is a good route** — the models complement each other.

## Related

- [[relational-model]], [[document-model]], [[graph-data-models]]
- [[normalization-and-denormalization]] — why many-to-one forces relational/joins
- [[schema-on-read-vs-schema-on-write]] · [[data-locality]]
- [[ch02-data-models-and-query-languages]]

## Sources

DDIA Ch 2 ("Relational Model Versus Document Model"). Refs: Codd 1970
([[codd-relational-model]]); Stonebraker & Hellerstein "What Goes Around Comes
Around" ([[what-goes-around-comes-around]]).
