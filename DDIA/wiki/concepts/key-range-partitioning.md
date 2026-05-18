---
title: Key-Range Partitioning
type: concept
chapters: [6]
tags: [partitioning, key-range, range-scan]
status: developing
updated: 2026-05-16
---

# Key-Range Partitioning

## Definition

Assign a **continuous range of keys** (min→max) to each partition, like volumes of a
print encyclopedia. Knowing the boundaries → know the partition → route directly.
Ranges are **not evenly spaced** (data isn't) — boundaries must adapt to the data
(chosen by admin or DB). *(DDIA Ch 6)*

## Properties

- Keys kept **sorted** within a partition ([[sstables-and-lsm-trees]]) → **efficient
  range scans**; a key can act as a concatenated index (one-to-many fetches).
- Used by Bigtable, HBase, RethinkDB, MongoDB <2.4.
- Rebalanced by **dynamic** splitting (see [[rebalancing-partitions]]).

## The hot-spot downside

Sequential keys cause hot spots: a timestamp key ⇒ all of today's sensor writes hit
one partition while others idle. Fix: prefix with another field (sensor name) so the
partition key is e.g. `(sensor, timestamp)` — then a multi-sensor time-range query
needs a separate range query per sensor.

## Related concepts

- [[hash-partitioning]] · [[key-range-vs-hash-partitioning]] ·
  [[skewed-workloads-and-hot-spots]] · [[rebalancing-partitions]]

## Sources

DDIA Ch 6 ("Partitioning by Key Range").
