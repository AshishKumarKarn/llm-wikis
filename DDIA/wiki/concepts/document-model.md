---
title: Document Model
type: concept
chapters: [2]
tags: [data-models, document, nosql, json]
status: solid
updated: 2026-05-16
---

# Document Model

## Definition

A data model storing self-contained documents (typically JSON/XML or a binary
variant like BSON), where one-to-many relationships are represented by **nesting**
records within their parent rather than in separate tables. Implemented by MongoDB,
RethinkDB, CouchDB, Espresso. *(DDIA Ch 2)*

## Why it matters

It is the NoSQL answer to the [[object-relational-impedance-mismatch]] and the
shredding of tree-shaped data across many relational tables. Driven by demands for
scalability, open source, specialized queries, and schema dynamism (the "Birth of
NoSQL", #NoSQL 2009 → "Not Only SQL"; *polyglot persistence*).

## How it works

- A document (e.g. a LinkedIn résumé) keeps its one-to-many sub-items (positions,
  education, contact_info) in one place → strong **[[data-locality]]**, one query to
  fetch the whole tree.
- **Reverted to the hierarchical (IMS) model** for nesting — but did **not** repeat
  CODASYL: many-to-one/many-to-many use a *document reference* (≈ foreign key)
  resolved by join/follow-up queries at read time.
- Typically **[[schema-on-read-vs-schema-on-write|schema-on-read]]**: no enforced
  schema; structure is implicit in the reading code.

## Trade-offs

- **Strength:** tree-shaped, self-contained data; schema flexibility; closeness to
  app objects.
- **Weakness:** weak join support; many-to-many is awkward; can't reference a nested
  item directly (must say "2nd item in positions"); whole-document load/rewrite is
  wasteful for large docs (keep documents small). Data tends to grow interconnected,
  eroding the fit.
- Full head-to-head: [[relational-vs-document-model]].

## Related concepts

- [[relational-model]] · [[graph-data-models]] (for highly interconnected data)
- [[object-relational-impedance-mismatch]] · [[data-locality]]
- [[schema-on-read-vs-schema-on-write]]

## Sources

DDIA Ch 2 ("Relational Model Versus Document Model", "Schema flexibility").
