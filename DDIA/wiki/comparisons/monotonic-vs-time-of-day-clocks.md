---
title: "Monotonic vs. Time-of-Day Clocks"
type: comparison
chapters: [8]
tags: [distributed, clocks, comparison]
status: solid
updated: 2026-05-16
---

# Monotonic vs. Time-of-Day Clocks

Modern computers have (at least) two clocks serving different purposes. *(DDIA Ch 8)*

| | Time-of-day clock | Monotonic clock |
|---|---|---|
| Returns | wall-clock date/time (since epoch) | arbitrary always-increasing value |
| API | `clock_gettime(CLOCK_REALTIME)`, `System.currentTimeMillis()` | `clock_gettime(CLOCK_MONOTONIC)`, `System.nanoTime()` |
| Synced | yes (NTP) | no (NTP only *slews* rate, ≤0.05%, never jumps) |
| Can jump back | **yes** (NTP reset) | no (guaranteed forward) |
| Cross-machine comparable | yes (ideally) | **no** (meaningless absolute value) |
| Use for | points in time (timestamps, expiry) | **durations** (timeouts, response times) |
| Resolution | historically coarse (10 ms old Windows) | usually µs or better |

## The rule

Use **monotonic** clocks for measuring elapsed time in distributed systems — they
don't assume cross-node sync and tolerate measurement inaccuracy. Use **time-of-day**
only for actual calendar timestamps, knowing they can jump and disagree across nodes
(why LWW timestamp ordering is dangerous — see [[unreliable-clocks]]). For *ordering*
events, prefer **logical clocks** over either physical clock.

## Related

- [[unreliable-clocks]] · [[happens-before-and-concurrency]] (logical clocks) ·
  [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 8 ("Monotonic Versus Time-of-Day Clocks").
