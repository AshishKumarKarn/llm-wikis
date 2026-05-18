---
title: Thrift & Protocol Buffers
type: concept
chapters: [4]
tags: [encoding, serialization, schema, thrift, protobuf]
status: solid
updated: 2026-05-16
---

# Thrift & Protocol Buffers

## Definition

Binary encoding libraries requiring a **schema** (defined in an IDL). Protocol
Buffers from Google, Thrift from Facebook (both open-sourced 2007–08). A code
generator turns the schema into classes in many languages. *(DDIA Ch 4)*

## How it works

- The encoded record is a concatenation of fields. Each field carries a **field tag**
  (the number in the schema) + datatype annotation; **field names are not encoded**
  (hence compact: ~33 bytes vs. 81 textual JSON). Unset fields are omitted.
- Thrift has BinaryProtocol (59 B) and the tighter CompactProtocol (34 B —
  type+tag packed in a byte, variable-length ints). Protobuf (33 B) is similar.
- `required`/`optional` don't change the bytes; `required` only adds a runtime
  presence check.

## Schema evolution via field tags

- **Change a field's name** — fine (names aren't encoded). **Change its tag** —
  breaks all existing data.
- **Add a field**: give it a new tag. Old code ignores unknown tags (datatype
  annotation says how many bytes to skip) → **forward compat**. New field must be
  **optional or have a default** (else new `required` field breaks **backward
  compat** when old code didn't write it).
- **Remove a field**: only an optional one; never reuse its tag.
- **Change datatype**: possibly, with precision/truncation risk (32→64-bit int: old
  code reading new data may truncate).
- Protobuf has no list type — a `repeated` field is the tag appearing multiple times,
  so `optional` → `repeated` is a safe evolution. Thrift has a real (nestable) list
  type but lacks that particular evolution.

## Trade-offs

Compact + statically-typed codegen, but **tags must be hand-assigned** — awkward for
dynamically generated schemas (contrast [[avro]], which uses names).

## Related concepts

- [[avro]] · [[schema-evolution]] · [[data-encoding-formats]] ·
  [[backward-forward-compatibility]]
- [[rpc-vs-rest]] — gRPC (Protobuf), Thrift/Finagle RPC

## Sources

DDIA Ch 4 ("Thrift and Protocol Buffers").
