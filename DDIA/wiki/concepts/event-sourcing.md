---
title: Event Sourcing
type: concept
chapters: [11]
tags: [stream, event-sourcing, ddd, derived-data]
status: solid
updated: 2026-05-16
---

# Event Sourcing

## Definition

Storing all changes to application state as a log of **immutable, append-only events**
modeled at the **application level** (from domain-driven design). Updates/deletes
discouraged. *(DDIA Ch 11)*

## vs. Change Data Capture

[[change-data-capture|CDC]] extracts low-level row changes the app needn't know
about; **event sourcing** events express the **intent of a user action** ("student
cancelled enrollment") not the resulting state mutations ("deleted from enrollments,
added to feedback"). Application-level events are more meaningful, ease evolution,
aid debugging/auditing, and let new side effects chain off existing events. Similar
to the chronicle data model / a star-schema fact table. (Tool-agnostic; Event Store
exists, but any DB/log-broker works.) Full comparison: [[cdc-vs-event-sourcing]].

## Deriving current state

Users want current state, not history → apps replay the event log into a
read-optimized view (**deterministic** so replay is reproducible). **Log compaction
differs**: CDC events carry the whole new record (compactable); event-sourcing events
express intent and don't override prior events → need the **full history** (snapshots
are only a perf optimization).

## Commands vs. events

A user request is first a **command** — may still **fail** validation (username/seat
taken — the [[consensus|uniqueness]] problem). If validated & accepted it becomes an
**event**: durable, immutable, a **fact**. A later cancellation is a *separate*
event. A consumer **cannot reject** an event (already seen by others) → validation
must be **synchronous** before the event (serializable transaction, or split into
tentative + confirmation events — cf. [[total-order-broadcast]]).

## Related concepts

- [[change-data-capture]] · [[cdc-vs-event-sourcing]] ·
  [[state-streams-immutability]] · [[log-based-message-brokers]] ·
  [[systems-of-record-and-derived-data]]

## Sources

DDIA Ch 11 ("Event Sourcing").
