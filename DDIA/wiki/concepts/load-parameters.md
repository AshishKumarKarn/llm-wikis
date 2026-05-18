---
title: Load Parameters & the Twitter Fan-out Case Study
type: concept
chapters: [1]
tags: [scalability, performance, case-study]
status: solid
updated: 2026-05-16
---

# Load Parameters & the Twitter Fan-out Case Study

## Definition

**Load parameters** are the few numbers that succinctly describe the current load on
a system. The right parameters depend on the architecture: requests/sec, read/write
ratio, simultaneously active users, cache hit rate, or a distribution (e.g. followers
per user). Sometimes the average matters; sometimes a few extreme cases dominate the
bottleneck. *(DDIA Ch 1)*

## Why it matters

You cannot discuss [[scalability]] ("what if load doubles?") until load is
quantified. Choosing the *wrong* parameter hides the real bottleneck.

## The Twitter case study (canonical example)

Twitter, Nov 2012 data:

- Post tweet: 4.6k req/s avg, 12k peak.
- Home timeline read: **300k req/s**.

Raw write volume (12k/s) is trivial. The real load parameter is **fan-out**: the
distribution of followers per user.

- **Approach 1 — fan-out on read.** Posting just inserts into a global tweet
  collection. A timeline read does a JOIN over follows + tweets, merged by time.
  Cheap writes, expensive reads. Twitter's first version; it could not keep up with
  300k timeline reads/s.
- **Approach 2 — fan-out on write.** Maintain a per-user "mailbox" cache; on post,
  insert the tweet into every follower's timeline cache. Reads are precomputed and
  cheap. Cost: 4.6k posts/s × ~75 followers ≈ **345k writes/s** to caches, and a
  celebrity with 30M followers triggers 30M writes for one tweet (within a 5 s SLA).
- **Approach 3 — hybrid.** Most users fanned out on write; celebrities excepted and
  fetched on read, merged at read time. Delivers consistently good performance.
  Revisited in [[ch12-the-future-of-data-systems]].

The lesson: the dominant load parameter (follower distribution / fan-out) determined
the architecture, and the right answer was *do more work at write time because reads
outnumber writes ~100:1*.

## Trade-offs

- Write-time work vs. read-time work — shift cost to whichever is rarer.
- Average vs. tail of the distribution — averages ("75 followers") hide the
  celebrity skew that actually breaks the design.

## Related concepts

- [[scalability]] — the goal load parameters serve
- [[response-time-percentiles]] — the matching performance measure
- Materialized-view / precompute parallels → [[ch11-stream-processing]]

## Sources

DDIA Ch 1 ("Describing Load"). Data: Raffi Krikorian, "Timelines at Scale", QCon SF
2012.
