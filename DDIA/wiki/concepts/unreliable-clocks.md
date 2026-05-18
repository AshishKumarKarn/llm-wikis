---
title: Unreliable Clocks
type: concept
chapters: [8]
tags: [distributed, clocks, ordering, lww, truetime]
status: solid
updated: 2026-05-16
---

# Unreliable Clocks

## Definition

Each machine has its own quartz clock (drifts with temperature), loosely synced via
**NTP**. Communication isn't instantaneous, so ordering events across nodes is hard.
Clocks "seem simple but have a surprising number of pitfalls". *(DDIA Ch 8)*

## Two kinds (see [[monotonic-vs-time-of-day-clocks]])

- **Time-of-day** (wall clock, NTP-synced): can jump backward/forward; unsuitable
  for measuring elapsed time.
- **Monotonic**: only moves forward; for durations/timeouts; absolute value
  meaningless; not comparable across machines.

## Clock sync is fickle

Quartz drift (~200 ppm → 17 s/day if synced daily); forced resets (time jumps);
silent NTP firewall misconfig; NTP accuracy ≤ network delay (~35 ms+ over internet,
spikes to seconds); wrong NTP servers; **leap seconds** crash systems; VM clock
jumps; untrusted device clocks. High accuracy (GPS/PTP, ~100 µs for MiFID II) is
possible but expensive. Incorrect clocks **fail silently** → subtle data loss, not a
crash → monitor clock offsets, evict drifted nodes.

## The danger: timestamps for ordering events

Using time-of-day timestamps to order writes (LWW) is **dangerous**: a write that
happened *causally later* can get an *earlier* timestamp (clock skew) → the later
write is silently dropped, data lost arbitrarily. LWW can't distinguish sequential
from truly concurrent writes (needs causality tracking — version vectors,
[[happens-before-and-concurrency]]). NTP can't be made accurate enough (limited by
round-trip time). **Logical clocks** (incrementing counters) are the safe alternative
for *ordering* (vs. physical clocks for elapsed time) → [[ch09-consistency-and-consensus]].

## Confidence intervals & global snapshots

A clock reading is a *range*, not a point — but most APIs (`clock_gettime`) don't
expose the error. Exception: Google **[[google-spanner|Spanner]] TrueTime** returns
`[earliest, latest]`; Spanner **waits out the confidence interval** before committing
so causally-later transactions get non-overlapping intervals → distributed
[[snapshot-isolation]] (needs GPS/atomic clocks per datacenter, ~7 ms).

## Related concepts

- [[monotonic-vs-time-of-day-clocks]] · [[process-pauses]] ·
  [[happens-before-and-concurrency]] · [[write-conflict-resolution]]
- [[google-spanner]] · [[lamport-clocks-paper]] · [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 8 ("Unreliable Clocks").
