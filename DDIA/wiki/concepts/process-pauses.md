---
title: Process Pauses
type: concept
chapters: [8]
tags: [distributed, gc, leases, real-time]
status: solid
updated: 2026-05-16
---

# Process Pauses

## Definition

A node's thread can be **paused for an unbounded time at any point**, even mid-
function, without noticing — the rest of the world keeps moving and may declare it
dead. *(DDIA Ch 8)*

## Causes

Stop-the-world **GC pauses** (sometimes minutes; even "concurrent" collectors pause);
**VM suspend/resume** (live migration); laptop lid close; OS context switch /
hypervisor steal time; synchronous disk I/O (incl. surprise: Java lazy classloading);
**paging/swap thrashing**; `SIGSTOP` (accidental Ctrl-Z). Analogous to thread-safety
on one machine — but distributed systems have **no shared memory**, only unreliable
messages, so mutexes/semaphores don't translate.

## The lease/lock bug

A leader holds a **lease** (lock with timeout), checks `lease.isValid()` then
processes a request. If a pause occurs between the check and the action, the lease
may have expired and another node taken over → two leaders write → corruption
(real HBase bug). Relying on synchronized clocks for the expiry makes it worse. Fix:
[[fencing-tokens]].

## Real-time guarantees

Pauses *can* be eliminated with **hard real-time** systems (RTOS, bounded library
times, restricted allocation, huge testing) — but very expensive, restricts
languages/tools, and "real-time" ≠ "high-performance" (lower throughput). Only worth
it for safety-critical embedded (airbags). Server data systems instead **mitigate**:
treat GC as a planned brief outage (drain requests, GC, rejoin — used by latency-
sensitive trading), or restart processes before long-lived-object GC (rolling).

## Related concepts

- [[unreliable-clocks]] · [[unreliable-networks]] · [[fencing-tokens]] ·
  [[truth-by-majority]]
- [[failover-and-split-brain]] · [[serializable-snapshot-isolation]] (the
  "outdated premise" parallel)

## Sources

DDIA Ch 8 ("Process Pauses").
