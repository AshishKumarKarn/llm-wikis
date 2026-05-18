---
title: Apache Avro
type: concept
chapters: [4]
tags: [encoding, serialization, schema, avro, hadoop]
status: solid
updated: 2026-05-16
---

# Apache Avro

## Definition

A binary schema-driven encoding (2009, a Hadoop subproject — Thrift didn't fit
Hadoop). Two schema languages: Avro IDL (human) and a JSON-based one (machine). The
example record encodes in **just 32 bytes** — the most compact seen. *(DDIA Ch 4)*

## How it works

- **No field tags, no field names in the data** — just concatenated values. A string
  is length + UTF-8 bytes with nothing marking it as a string. Therefore the data can
  *only* be decoded by code that knows the exact schema used to write it.

## Writer's vs. reader's schema (the key idea)

The **writer's schema** (used to encode) and the **reader's schema** (the code's
expectation) need not be identical — only **compatible**. The Avro library resolves
differences side by side: fields matched **by name**; field in writer but not reader
→ ignored; field in reader but not writer → filled with the reader's **default
value**; field order irrelevant.

- **Forward compat**: new-schema writer, old-schema reader. **Backward compat**:
  new-schema reader, old-schema writer.
- May only add/remove a field **with a default value**. Nullable requires an explicit
  **union type** (`union { null, long }`); default must match the first union branch.
  Renaming a field: reader aliases (backward but not forward compatible). Adding a
  union branch: backward but not forward compatible.

## How the reader learns the writer's schema

Not embedded per record (would dwarf the data). Instead: **object container file** —
schema once at file start (large Hadoop files of uniform records); **DB record** —
version number per record + a schema-version registry (e.g. Espresso); **network
connection** — negotiated at setup (Avro RPC).

## Why Avro wins for dynamic schemas

No tags ⇒ trivially generate an Avro schema from a relational schema and re-generate
on DB schema change (fields matched by name). Optional codegen — usable from
dynamically-typed languages / Apache Pig without it (object container files are
self-describing).

## Related concepts

- [[thrift-and-protocol-buffers]] — the tag-based contrast
- [[schema-evolution]] · [[backward-forward-compatibility]] ·
  [[data-encoding-formats]]
- [[ch10-batch-processing]] — Avro container files in Hadoop/archival

## Sources

DDIA Ch 4 ("Avro").
