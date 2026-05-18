---
title: "Schema-on-Read vs. Schema-on-Write"
type: comparison
chapters: [2, 4]
tags: [data-models, schema, comparison]
status: solid
updated: 2026-05-16
---

# Schema-on-Read vs. Schema-on-Write

## The distinction

- **Schema-on-write** (traditional relational): schema is explicit; the DB enforces
  that all written data conforms. ≈ static (compile-time) type checking.
- **Schema-on-read** ("schemaless" — a misleading term): structure is *implicit*,
  interpreted only when data is read; the reading code assumes a structure the DB
  does not enforce. ≈ dynamic (runtime) type checking. *(DDIA Ch 2)*

## Why it matters

"Schemaless" document DBs still have a schema — just an unenforced, implicit one.
The real, contentious question (like the static-vs-dynamic-typing debate) is *where*
the schema lives and *when* it is checked. No universal right answer.

## Behavior on a format change (the worked example)

Splitting `name` into `first_name`/`last_name`:

- **Schema-on-read:** start writing new docs with the new fields; reading code
  handles old docs (`if (user.name && !user.first_name) …`). No migration.
- **Schema-on-write:** `ALTER TABLE … ADD COLUMN` (usually milliseconds; **MySQL is
  the notable exception** — copies the whole table, minutes–hours of downtime; tools
  like gh-ost / pt-online-schema-change work around it) + a backfill `UPDATE`
  (slow on big tables; can instead default NULL and fill at read time, like
  schema-on-read).

## When each is advantageous

- **Schema-on-read:** heterogeneous data — many object types impractical to give
  each its own table, or structure dictated by uncontrolled external systems.
- **Schema-on-write:** records expected to share structure — schema documents and
  enforces it.

## Related

- [[document-model]] · [[relational-model]] · [[relational-vs-document-model]]
- [[ch04-encoding-and-evolution]] — schema *evolution* over time, the deep follow-up

## Sources

DDIA Ch 2 ("Schema flexibility in the document model"); deepened in Ch 4. Ref:
Awadallah, "Schema-on-Read vs. Schema-on-Write" (2009).
