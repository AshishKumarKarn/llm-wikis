---
title: MapReduce Querying
type: concept
chapters: [2, 10]
tags: [query-languages, mapreduce, distributed, functional]
status: developing
updated: 2026-05-16
---

# MapReduce Querying

## Definition

A programming model for bulk data processing across many machines (popularized by
Google), available in limited form in some NoSQL stores (MongoDB, CouchDB) for
read-only queries across many documents. It is **neither declarative nor fully
imperative** — query logic is expressed as snippets of code (`map`/`reduce`,
borrowed from functional programming's collect/fold) called repeatedly by the
framework. *(DDIA Ch 2)*

## How it works

- `map` is called once per matching document, `emit`-ting key→value pairs.
- The framework groups emitted values by key; `reduce` is called once per key to fold
  the values (e.g. sum).
- Example: shark sightings per month — `map` emits `("1995-12", n)`; `reduce("1995-12",
  [3,4]) → 7`. Equivalent SQL is a `WHERE … GROUP BY` aggregate.

## Constraints (and why they exist)

`map`/`reduce` must be **pure functions**: only their input, no extra DB queries, no
side effects. Purity lets the DB run them anywhere, in any order, and **rerun on
failure** — the foundation for distributed execution and fault tolerance (deepened in
[[ch10-batch-processing]]).

## Trade-offs

- Powerful (parse, call libraries, compute) but a **usability problem**: two
  carefully coordinated functions are harder than one query, and a declarative
  language gives the optimizer more room. ⇒ MongoDB 2.2 added the **declarative
  aggregation pipeline** — "a NoSQL system may accidentally reinvent SQL in disguise."
- MapReduce is low-level; SQL can compile to a pipeline of MapReduce ops, but SQL
  isn't single-machine-bound and MapReduce has no monopoly on distributed execution.

## Related concepts

- [[declarative-vs-imperative-queries]] — MapReduce sits between
- [[ch10-batch-processing]] — full treatment; [[mapreduce-paper]]
- [[graph-data-models]] — Pregel is the graph-processing analogue

## Sources

DDIA Ch 2 ("MapReduce Querying"); [[mapreduce-paper]] (Dean & Ghemawat, OSDI 2004);
deep dive Ch 10.
