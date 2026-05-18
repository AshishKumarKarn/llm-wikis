---
title: Designing Applications Around Dataflow
type: concept
chapters: [12]
tags: [dataflow, derived-data, architecture]
status: solid
updated: 2026-05-16
---

# Designing Applications Around Dataflow

The "database inside-out" design pattern: compose unbundled storage/processing with
application code around dataflow. Akin to spreadsheets (a formula auto-recalculates
when inputs change) — but fault-tolerant, scalable, durable, and integrating
heterogeneous tools. *(DDIA Ch 12)*

## Application code as a derivation function

Derived datasets come from a transformation: secondary index (sort by indexed
field), full-text index (NLP + inverted index), ML model (feature extraction over
training data), cache (UI-shaped aggregation). Standard ones are built in (`CREATE
INDEX`); custom ones need application code — where DBs struggle (triggers/SPs are an
afterthought).

## Separation of code and state

DBs are poor general code-deployment environments (no good package mgmt, version
control, rolling upgrades) → specialize: durable storage *vs.* application code
(Mesos/K8s). "Separation of Church and state" — **stateless services + state in a
database**. But you can't *subscribe* to a mutable variable; DBs inherited this
passive model (poll, not notify) — subscribing to changes is only just emerging
([[change-data-capture|API support for change streams]]).

## Dataflow: state changes ↔ code

Treat the DB's change log as a stream to **subscribe** to (vs. passive variable).
App code responds to state changes by triggering other state changes — extend the
DB's internal trigger/index-maintenance idea to *external* derived systems via stream
processors. Maintaining derived data ≠ async job queues: **order matters** & **fault
tolerance is key** (one lost message → permanent divergence) — stringent demands, but
cheaper & more robust than distributed transactions.

## Stream processors vs. services

Microservices communicate by synchronous request/response; dataflow uses one-way
async streams → better fault tolerance *and* performance. Currency-conversion
example: instead of RPC to an exchange-rate service, **subscribe** to rate updates
and keep a local copy → a [[stream-joins|stream-table join]] (the fastest, most
reliable request is no request). Time-dependent (reprocessing needs historical
rates).

## Write path vs. read path (observing derived state)

The **write path** = precompute derived data eagerly when written (≈ eager
evaluation); the **read path** = compute on request (≈ lazy evaluation). Indexes/
caches/materialized views just **shift the boundary** between them (more write-time
work to save read-time work) — exactly the Twitter timeline write/read trade-off from
Ch 1 ("full circle", different for celebrities).

- **Stateful offline-capable clients**: on-device state = a cache; UI pixels = a
  materialized view of model objects = a local replica of server state.
- **Push state to clients**: extend the write path *to the end user* (server-sent
  events / WebSockets); offline → reconnect like a log consumer resuming from an
  offset. End-to-end event streams (Elm, React/Flux/Redux) — rethink
  request/response toward publish/subscribe.
- **Reads are events too**: route read queries through the stream processor → a
  stream-table join between queries and the database; logging reads aids
  causality/provenance tracking; enables **multi-partition** distributed query
  (Storm distributed RPC, fraud-scoring joins).

## Related concepts

- [[unbundling-databases]] · [[change-data-capture]] · [[event-sourcing]] ·
  [[materialized-views-and-data-cubes]] · [[load-parameters]] (Twitter timeline) ·
  [[stream-joins]] · [[request-routing]]

## Sources

DDIA Ch 12 ("Designing Applications Around Dataflow", "Observing Derived State").
