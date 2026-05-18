---
title: Monotonic Reads
type: concept
chapters: [5]
tags: [consistency, replication]
status: developing
updated: 2026-05-16
---

# Monotonic Reads

## Definition

A guarantee that a user, making **several reads in sequence**, will **not see time go
backward** — they won't read older data after having read newer data. Stronger than
[[eventual-consistency]], weaker than strong consistency. *(DDIA Ch 5)*

## The anomaly it fixes

A user reads from a fresh follower (sees user 1234's new comment), then refreshes and
hits a more-lagging follower (comment gone) — the second read observes an *earlier*
point in time. Confusing: the comment appears then disappears.

## Implementation

Ensure each user always reads from the **same replica** (e.g. chosen by a hash of
user ID, not randomly). Different users may use different replicas; reroute if that
replica fails.

## Related concepts

- [[replication-lag]] · [[eventual-consistency]] · [[read-after-write-consistency]] ·
  [[consistent-prefix-reads]]

## Sources

DDIA Ch 5 ("Monotonic Reads"). Ref: Terry, "Replicated Data Consistency Explained
Through Baseball" (2011).
