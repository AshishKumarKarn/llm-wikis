---
title: Reasoning About Time in Streams
type: concept
chapters: [11]
tags: [stream, time, windows, event-time]
status: solid
updated: 2026-05-16
---

# Reasoning About Time in Streams

## Event time vs. processing time

A window like "last 5 minutes" is surprisingly tricky. **Event time** = the timestamp
in the event (when it happened); **processing time** = the local clock when the
stream processor handles it. Using processing time is simple but **wrong under any
lag** — queueing, network faults, consumer restart, reprocessing. (Analogy: Star Wars
release order ≠ episode order.) Measuring rate by processing time after a restart
shows a false spike while the backlog is consumed (the real rate was steady). Batch
has the same issue but it's more visible in streaming. *(DDIA Ch 11)*

## "Knowing when you're ready"

With event-time windows you can never be sure all events for a window have arrived —
**straggler events** may still come (buffered, network-delayed). Options: (1) **ignore
stragglers** (track/alert on the dropped fraction); (2) **publish a correction**
(retract & re-emit the window). A special "no more events before time t" message can
trigger windows but is hard with multiple producers.

## Whose clock?

A mobile app used offline buffers events for hours/days → extreme stragglers; the
device clock can't be trusted (wrong/maliciously set). Mitigation: log **three
timestamps** — event time (device), send time (device), receive time (server) —
estimate the device↔server offset from (receive − send) and correct the event time.

## Window types

- **Tumbling** — fixed length, non-overlapping; each event in exactly one window.
- **Hopping** — fixed length, overlapping by a hop (smoothing); = aggregate adjacent
  tumbling windows.
- **Sliding** — all events within an interval of each other (buffer sorted by time).
- **Session** — no fixed duration; group a user's events until an inactivity gap
  (sessionization, cf. Ch 10 GROUP BY).

## Related concepts

- [[stream-processing-uses]] · [[stream-joins]] · [[unreliable-clocks]] (Ch 8) ·
  [[stream-fault-tolerance]]

## Sources

DDIA Ch 11 ("Reasoning About Time").
