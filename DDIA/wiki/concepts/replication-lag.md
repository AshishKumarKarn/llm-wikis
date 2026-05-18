---
title: Replication Lag
type: concept
chapters: [5]
tags: [replication, consistency, eventual-consistency]
status: solid
updated: 2026-05-16
---

# Replication Lag

## Definition

The delay between a write happening on the leader and being reflected on a follower.
Usually < 1 s, but can grow to seconds/minutes near capacity or under network
problems. *(DDIA Ch 5)*

## Why it matters

The **read-scaling architecture** (many async followers serve reads) only works with
**asynchronous** replication (sync to all followers ⇒ one outage halts the system).
But async followers serve **stale** data → [[eventual-consistency]]: the same query
on leader vs. follower can differ, converging only if writes stop. "Eventually" is
deliberately vague — *no bound* on how far a replica can lag.

## The three anomalies (and their guarantees)

When lag grows, three concrete problems and the consistency model that fixes each:

- User can't see their own write → **[[read-after-write-consistency]]**.
- Time appears to go backward across reads → **[[monotonic-reads]]**.
- Causally-ordered writes seen out of order (question before answer) →
  **[[consistent-prefix-reads]]**.

## The deeper point

Handling these in application code is complex and error-prone. **This is why
transactions exist**: a database providing stronger guarantees lets the application
be simpler. Many distributed databases abandoned transactions claiming async/eventual
consistency is inevitable at scale — "overly simplistic"; a nuanced view develops in
[[ch07-transactions]] and [[ch09-consistency-and-consensus]].

## Related concepts

- [[eventual-consistency]] · [[read-after-write-consistency]] ·
  [[monotonic-reads]] · [[consistent-prefix-reads]]
- [[single-leader-replication]] · [[ch07-transactions]] ·
  [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 5 ("Problems with Replication Lag").
