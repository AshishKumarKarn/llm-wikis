---
title: Data Encoding Formats
type: concept
chapters: [4]
tags: [encoding, serialization, json, binary]
status: solid
updated: 2026-05-16
---

# Data Encoding Formats

## Definition

**Encoding** (= serialization/marshalling) translates in-memory representations
(objects, structs, pointers) into a self-contained **byte sequence** for files or the
network; **decoding** (parsing/deserialization) is the reverse. (Unrelated to
encryption; the transaction sense of "serialization" is a separate clash — see
[[ch07-transactions]].) *(DDIA Ch 4)*

## The families

**Language-specific** (Java `Serializable`, Ruby `Marshal`, Python `pickle`, Kryo) —
convenient but: locked to one language; **security risk** (decoding instantiates
arbitrary classes → RCE); versioning/compat an afterthought; often bloated/slow.
*Use only for transient data.*

**Textual: JSON / XML / CSV** — widespread, human-ish-readable, great as
*cross-organization interchange* (agreement matters more than efficiency). Subtle
problems: number ambiguity (XML/CSV can't tell number vs. digit-string; JSON can't
tell int vs. float, no precision — integers > 2^53 break in JavaScript, e.g. Twitter
tweet IDs sent twice); no binary strings (Base64 hack, +33% size); optional, complex
schemas; CSV has no schema and vague escaping.

**Binary JSON variants** (MessagePack, BSON, …) — keep the JSON model but must still
embed field names → only a small size win (66 vs. 81 bytes for the example); "not
clear it's worth the loss of human-readability."

**Binary schema-driven** ([[thrift-and-protocol-buffers]], [[avro]]) — compact (omit
field names), clear compat semantics, schema as documentation + codegen. Downside:
not human-readable. Ideas predate this (ASN.1, 1984; still used for X.509/SSL certs).

## Trade-offs

Interchange/readability (textual) vs. compactness/efficiency/guarantees (binary
schema) — the bigger the data (TB+), the more the format choice matters.

## Related concepts

- [[thrift-and-protocol-buffers]] · [[avro]] · [[schema-evolution]]
- [[backward-forward-compatibility]] · [[modes-of-dataflow]]

## Sources

DDIA Ch 4 ("Formats for Encoding Data").
