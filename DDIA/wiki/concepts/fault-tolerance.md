---
title: Fault Tolerance (Fault vs. Failure)
type: concept
chapters: [1, 8]
tags: [reliability, fault-tolerance, distributed]
status: developing
updated: 2026-05-16
---

# Fault Tolerance (Fault vs. Failure)

## Definition

A **fault** is one component of the system deviating from its spec. A **failure** is
the system *as a whole* stopping to provide the required service to the user. A
**fault-tolerant** (or **resilient**) system anticipates faults and prevents them
from becoming failures. *(DDIA Ch 1)*

## Why it matters

This distinction is the conceptual spine of [[reliability]]. You can never reduce
fault probability to zero, so the entire design goal is *fault → failure
containment*: build reliable systems from unreliable parts. "Fault-tolerant" is
slightly misleading — you only ever tolerate *certain types* of faults (no budget
for surviving the Earth falling into a black hole).

## How it works (the mechanism)

- Identify the fault classes you will tolerate (hardware / software / human — see
  [[reliability]]) and design containment for each.
- **Deliberate fault injection.** Counterintuitively, *increasing* the fault rate on
  purpose — randomly killing processes — keeps the fault-tolerance machinery
  continually exercised, so it actually works when real faults strike. Many critical
  bugs stem from poor error handling that only injection reveals. Canonical example:
  **Netflix Chaos Monkey** / Simian Army.
- Self-checking invariants: a system that promises a guarantee (e.g. messages in =
  messages out) can continuously verify it and alert on discrepancy.

This concept deepens substantially in [[ch08-the-trouble-with-distributed-systems]]
(partial failure, unreliable networks/clocks) and is the motivation for replication
and consensus.

## Trade-offs

- **Prevent vs. tolerate** — tolerate by default; prevent only where damage is
  irreversible (security).
- Fault injection adds operational risk and tooling cost in exchange for confidence
  that recovery paths actually work.

## Related concepts

- [[reliability]] — the property fault tolerance delivers
- [[ch08-the-trouble-with-distributed-systems]] — partial failure in depth
- [[ch05-replication]], [[ch09-consistency-and-consensus]] — concrete mechanisms

## Sources

DDIA Ch 1 ("Reliability"); forward refs to Ch 8. Chaos Monkey: Netflix Simian Army.
