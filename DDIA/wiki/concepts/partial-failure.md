---
title: Partial Failure
type: concept
chapters: [8]
tags: [distributed, fault-tolerance, reliability]
status: solid
updated: 2026-05-16
---

# Partial Failure

## Definition

In a distributed system, some parts may be broken in unpredictable ways while others
work fine — a **partial failure**. It is **nondeterministic**: anything involving
multiple nodes + the network may sometimes work and sometimes fail, and *you may not
even know whether it succeeded* (message travel time is also nondeterministic). This
is the **defining characteristic** of distributed systems. *(DDIA Ch 8)*

## Single computer vs. distributed

A single computer is deliberately designed as an idealized deterministic model: an
internal fault ⇒ **total crash** (kernel panic) rather than wrong results, because
wrong results are confusing. Distributed software must instead confront physical
reality (Coda Hale's anecdote: PDU/switch failures, a truck into the HVAC…).

## Cloud vs. supercomputing (two philosophies)

- **Supercomputer/HPC**: reliable specialized hardware; on a node fault, checkpoint +
  stop the whole cluster + restart — escalate partial failure into total failure
  (like a single machine).
- **Cloud**: commodity machines (higher failure rate, cheaper at scale), online (no
  downtime), geo-distributed over slow/unreliable internet → must **build
  fault-tolerance into software**, tolerate failed nodes (rolling upgrades, kill &
  replace VMs).

## Building reliable from unreliable

An old idea (von Neumann): error-correcting codes over noisy channels; TCP over
unreliable IP. The composite is *more* reliable but **not unboundedly so** (TCP
hides loss/reorder but can't remove delay) — still useful: low-level faults handled,
remaining faults easier to reason about (the **end-to-end argument**, Part III).
"Suspicion, pessimism, and paranoia pay off" — deliberately trigger faults (Chaos
Monkey, see [[fault-tolerance]]).

## Related concepts

- [[unreliable-networks]] · [[unreliable-clocks]] · [[process-pauses]]
- [[reliability]] · [[fault-tolerance]] · [[truth-by-majority]] · [[system-models]]

## Sources

DDIA Ch 8 ("Faults and Partial Failures").
