---
title: Stream Joins
type: concept
chapters: [11]
tags: [stream, joins, scd]
status: solid
updated: 2026-05-16
---

# Stream Joins

New events can appear anytime, making joins harder than batch. Three types. *(DDIA
Ch 11)*

## Stream-stream join (window join)

Both inputs are activity-event streams; match related events within a **time
window** (e.g. a search and its click within 1 h, by session ID — for click-through
rate; you need *both* sides, not just clicks). The processor maintains **state**
(recent events indexed by join key); each event checks the other index. Click may
arrive before search, or never.

## Stream-table join (stream enrichment)

One input is activity events, the other a database. Enrich each event with profile
info. Remote DB lookup per event is slow/overloading → keep a **local copy** (hash
table / disk index, like a [[batch-joins|map-side hash join]]); keep it fresh via
**[[change-data-capture|CDC]]** on the DB changelog. Effectively a join between two
streams: activity events and profile updates (the table side = an infinite window,
newer versions overwrite older).

## Table-table join (materialized view maintenance)

Both inputs are DB changelogs; every change on one side joins the latest state of the
other → a stream of changes to the **materialized view** of the join. The Twitter
timeline cache: maintain per-user inbox on tweet send/delete and follow/unfollow —
exactly maintaining `SELECT … JOIN follows … GROUP BY` incrementally (the product
rule: (u·v)′ = u′v + uv′).

## Time-dependence & slowly changing dimensions

All three keep state from one input and query it from the other; **order matters**
(follow then unfollow ≠ reverse). Across streams/partitions there's no ordering
guarantee → which profile/tax-rate version do you join with? Undetermined order →
**nondeterministic** join (not rerunnable to the same result). Data-warehouse fix:
**slowly changing dimension (SCD)** — a unique version ID per change; the fact
references the version at the time → deterministic, but **prevents log compaction**
(all versions retained).

## Related concepts

- [[batch-joins]] (Ch 10) · [[change-data-capture]] ·
  [[reasoning-about-time-in-streams]] · [[materialized-views-and-data-cubes]] ·
  [[stream-fault-tolerance]]

## Sources

DDIA Ch 11 ("Stream Joins").
