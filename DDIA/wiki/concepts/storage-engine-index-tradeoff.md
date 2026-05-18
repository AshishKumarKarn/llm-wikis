---
title: The Storage-Engine Index Trade-off
type: concept
chapters: [3]
tags: [storage, indexes, performance]
status: solid
updated: 2026-05-16
---

# The Storage-Engine Index Trade-off

## Definition

An **index** is an additional structure *derived from the primary data* that acts as
a signpost to locate data efficiently. The fundamental trade-off: **well-chosen
indexes speed up read queries, but every index slows down writes** (the index must be
updated on every write). *(DDIA Ch 3)*

## Why it matters

It is *the* organizing tension of storage engines. Because indexes don't change query
results — only performance — databases don't index everything by default; the
developer/DBA chooses indexes from knowledge of the app's query patterns to maximize
read benefit without excess write overhead.

## The "world's simplest database"

Two Bash functions: `db_set` appends `key,value` to a file; `db_get` greps for the
last occurrence. Lessons:

- **Appending is the simplest, fastest possible write** (sequential I/O) — many real
  databases internally use a [[log-structured-storage|log]] (append-only sequence of
  records; *not* an application/text log).
- The naive read is **O(n)** — scan the whole file. To find a key efficiently you
  need an **index**. Search the same data several ways ⇒ several indexes.

## Trade-offs

- Reads vs. writes vs. storage space (later framed by the literature as the "RUM
  conjecture").
- Index maintenance overhead is paid on every write, forever.

## Related concepts

- [[log-structured-storage]] · [[hash-index]] · [[b-tree]] ·
  [[sstables-and-lsm-trees]]
- [[secondary-indexes]] — indexes beyond the primary key
- [[btree-vs-lsm-tree]] — where the trade-off plays out concretely

## Sources

DDIA Ch 3 ("Data Structures That Power Your Database").
