---
title: "Ch 2 — Data Models and Query Languages"
type: chapter
chapters: [2]
tags: [data-models, relational, document, graph, query-languages]
status: solid
updated: 2026-05-16
---

# Ch 2 — Data Models and Query Languages

## One-paragraph thesis

Data models are the most consequential design choice in software — they shape both
how code is written and how we think about the problem. Applications layer one model
on another (app objects → general-purpose model → bytes), each layer hiding the one
below. The chapter compares three living general-purpose models — **relational**,
**document**, **graph** — shows the relational-vs-document debate is a re-run of the
1970s hierarchical (IMS) vs. network (CODASYL) vs. relational "great debate", and
argues query languages should be **declarative**. The recurring lesson: no
one-size-fits-all model; pick the one whose relationship structure matches your data.
*(DDIA Ch 2)*

## Key ideas

- **[[data-model-layering]]** — nested abstractions let database vendors and app
  developers work independently.
- **[[relational-vs-document-model]]** — the central comparison: document wins on
  schema flexibility, [[data-locality]], and matching app structures for tree-shaped
  data; relational wins on joins and many-to-one/many-to-many relationships.
- **[[object-relational-impedance-mismatch]]** — why ORMs exist and what they can't
  hide.
- **[[normalization-and-denormalization]]** — store human-meaningful data once
  (ID vs. duplicated text); normalization needs many-to-one relationships → joins.
- **History rhymes**: hierarchical model (IMS) ≈ today's document model;
  [[relational-model]] beat the **network/CODASYL** model by replacing
  hand-coded *access paths* with an automatic query optimizer. Document DBs
  reverted to nesting but did *not* repeat CODASYL — they still use references
  resolved at read time.
- **[[schema-on-read-vs-schema-on-write]]** — "schemaless" is really schema-on-read
  (implicit, dynamic) vs. schema-on-write (explicit, static).
- **[[declarative-vs-imperative-queries]]** — declarative (SQL, CSS, relational
  algebra) hides execution, enables optimization and parallelism.
- **[[mapreduce-querying]]** — between declarative and imperative; pure map/reduce
  functions; NoSQL "accidentally reinvents SQL" (MongoDB aggregation pipeline).
- **[[graph-data-models]]** — property graphs (Cypher/Neo4j), triple-stores
  (SPARQL/RDF), and Datalog; best when *anything* may relate to *everything*.

## Concepts introduced

- [[data-model-layering]] · [[relational-model]] · [[document-model]]
- [[object-relational-impedance-mismatch]] · [[normalization-and-denormalization]]
- [[data-locality]] · [[mapreduce-querying]] · [[graph-data-models]]

## Comparisons introduced

- [[relational-vs-document-model]]
- [[schema-on-read-vs-schema-on-write]]
- [[declarative-vs-imperative-queries]]

## Systems / papers referenced

- [[postgresql]], [[mongodb]], [[neo4j]] — relational/document/graph exemplars
- [[google-spanner]], [[cassandra]] — locality in a relational/column-family model
- [[codd-relational-model]] (1970) · [[what-goes-around-comes-around]] (Stonebraker
  & Hellerstein) · [[mapreduce-paper]] · [[bigtable-paper]]

## Trade-offs & tensions

- **Document vs. relational** — locality & schema flexibility vs. join support; data
  *tends to become more interconnected* over time, eroding the document fit.
- **Schema-on-read vs. -on-write** — flexible heterogeneous data vs. documented,
  enforced structure (like dynamic vs. static typing).
- **Locality** — fast whole-document reads vs. wasteful loads/rewrites of large
  documents on partial access.
- **Declarative vs. imperative** — less control but enables optimizer + parallelism.
- Convergence: relational DBs add JSON/XML; document DBs add joins — the models are
  merging, "a hybrid is a good route."

## Connections to other chapters

- Storage-engine implementation of these models → [[ch03-storage-and-retrieval]].
- Schema *evolution* (schema-on-read/write over time, rolling upgrades) →
  [[ch04-encoding-and-evolution]].
- MapReduce in depth → [[ch10-batch-processing]]; Pregel graph processing too.
- Normalization/denormalization/derived data revisited → Part III.
- Fault-tolerance & concurrency differences deferred → [[ch05-replication]],
  [[ch07-transactions]].

## Open questions / things to revisit

- How does schema-on-read interact with the encoding-evolution machinery of Ch 4?
- Where do column-family models (Bigtable/Cassandra) sit once Ch 3 covers storage?
