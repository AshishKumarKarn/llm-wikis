---
title: Schema Evolution & the Merits of Schemas
type: concept
chapters: [4]
tags: [encoding, schema, evolvability]
status: solid
updated: 2026-05-16
---

# Schema Evolution & the Merits of Schemas

## Definition

**Schema evolution** = changing a data schema over time while preserving
[[backward-forward-compatibility]]. *(DDIA Ch 4)*

## The merits of (binary) schemas

Thrift/Protobuf/Avro schema languages are far simpler than XML/JSON Schema (no
regex/range validation) and so support many languages. Properties:

- **More compact** than binary-JSON (field names omitted).
- **Schema is documentation that can't go stale** — it's *required* to decode, so
  it's always current (unlike hand-maintained docs).
- A **schema registry** lets you check forward/backward compatibility *before*
  deploying.
- **Codegen** enables compile-time type checking in statically typed languages.

> Conclusion: schema evolution gives the same flexibility as
> schemaless/[[schema-on-read-vs-schema-on-write|schema-on-read]] JSON, **plus**
> better data guarantees and tooling.

## Per-format rules (summary)

| | [[thrift-and-protocol-buffers]] | [[avro]] |
|---|---|---|
| Identity | hand-assigned **field tags** | field **names** |
| Add field | new tag; must be optional/default | must have a default |
| Remove field | optional only; never reuse tag | must have a default |
| Rename field | free (tags carry meaning) | reader aliases (backward-only) |
| Dynamic schemas | awkward (manual tags) | natural |

Datatype changes: possible with precision/truncation risk in all.

## Related concepts

- [[backward-forward-compatibility]] · [[thrift-and-protocol-buffers]] · [[avro]]
- [[schema-on-read-vs-schema-on-write]] — the Ch 2 axis this resolves
- [[modes-of-dataflow]] — where the rules get applied

## Sources

DDIA Ch 4 ("Field tags and schema evolution", "The Merits of Schemas").
