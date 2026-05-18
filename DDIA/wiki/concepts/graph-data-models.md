---
title: Graph Data Models
type: concept
chapters: [2]
tags: [data-models, graph, cypher, sparql, datalog]
status: solid
updated: 2026-05-16
---

# Graph Data Models

## Definition

A model of **vertices** (nodes/entities) and **edges** (relationships/arcs), best
when many-to-many relationships are common and *anything is potentially related to
everything*. Examples: social graphs, the web graph, road/rail networks (shortest
path, PageRank). Graphs can be homogeneous or store many different vertex/edge types
in one store (Facebook's single social graph). *(DDIA Ch 2)*

## Why it matters

For highly interconnected data the [[document-model]] is awkward and the
[[relational-model]] only acceptable; graphs are the **most natural**, and are good
for **evolvability** — new features extend the graph without schema upheaval.

## The two model variants

**Property graph** (Neo4j, Titan, InfiniteGraph): each vertex has id, in/out edges,
properties; each edge has id, tail+head vertex, a **label**, properties. Modelable as
two relational tables (vertices, edges) with indexes on both head & tail. Any vertex
can link to any other (no schema restriction); traverse forward and backward.

**Triple-store** (Datomic, AllegroGraph): all data as `(subject, predicate, object)`.
Object is either a literal (→ a property) or another vertex (→ predicate is an edge
label). Turtle/N3 is the readable RDF syntax; **RDF/semantic web** uses URIs as
subjects/predicates so independently-published data composes without name clashes
(semantic web was overhyped, but triples are a fine *internal* model).

## Query languages

- **Cypher** (Neo4j) — declarative; arrow pattern `(a)-[:REL]->(b)`; `:WITHIN*0..`
  = variable-length traversal (regex-`*`-like).
- **SPARQL** (RDF triple-stores) — predates Cypher; even more concise; Cypher
  borrowed its pattern matching.
- **Datalog** (Datomic, Cascalog/Hadoop) — older, subset of Prolog; define
  **rules** (derived predicates, can recurse) and build complex queries from reusable
  pieces. Different mindset; better for complex data, less convenient for one-offs.
- The same "US → Europe emigrants" query is ~4 lines in Cypher/SPARQL vs. ~29 lines
  of SQL `WITH RECURSIVE` CTEs — illustrates models target different use cases.

## Graph DBs are *not* CODASYL redux

Unlike the network model: no schema restricting which vertex links to which; direct
access by vertex id or index (no mandatory access-path traversal); vertices/edges
unordered; high-level declarative query languages instead of brittle imperative code.

## Related concepts

- [[relational-vs-document-model]] · [[normalization-and-denormalization]]
- [[declarative-vs-imperative-queries]] — Cypher/SPARQL/Datalog are declarative
- [[ch10-batch-processing]] — Pregel-style graph processing frameworks

## Sources

DDIA Ch 2 ("Graph-Like Data Models"). Refs: Neo4j manual; SPARQL 1.1; Datalog
surveys; Facebook TAO.
