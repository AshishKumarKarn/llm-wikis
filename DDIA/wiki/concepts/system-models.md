---
title: System Models (Safety & Liveness)
type: concept
chapters: [8]
tags: [distributed, formal-methods, correctness]
status: solid
updated: 2026-05-16
---

# System Models (Safety & Liveness)

## Definition

A **system model** is an abstraction stating which faults an algorithm may assume, so
algorithms can be proved correct independent of hardware/software details. *(DDIA
Ch 8)*

## Timing models

- **Synchronous** — bounded network delay, process pauses, clock error. Unrealistic
  (unbounded delays/pauses do occur).
- **Partially synchronous** — synchronous *most* of the time, occasionally exceeds
  bounds. **The realistic model for most systems.**
- **Asynchronous** — no timing assumptions, not even a clock (no timeouts). Very
  restrictive.

## Node-failure models

- **Crash-stop** — a node fails only by crashing, gone forever.
- **Crash-recovery** — nodes may crash and later recover; **stable storage** survives,
  in-memory state lost. *(Most useful model: partially-synchronous + crash-recovery.)*
- **Byzantine** — nodes may do anything, including lying ([[byzantine-faults]]).

## Correctness: safety vs. liveness

An algorithm's correctness = properties it always satisfies in its system model
(e.g. fencing tokens: *uniqueness*, *monotonic sequence*, *availability*).

- **Safety** — "nothing bad happens"; violation is at a *specific point* and
  *irreversible*. Required to hold **always, in all situations** (even if everything
  crashes).
- **Liveness** — "something good eventually happens"; may not hold now but can later.
  Often contains "eventually" (eventual consistency is a liveness property). Allowed
  **caveats** (e.g. respond *if* a majority is up and the network *eventually*
  recovers).

## Map ≠ territory

Models are simplified abstractions: real disks corrupt, nodes get "amnesia" breaking
quorum assumptions, "impossible" cases still need handling (`printf("Sucks to be
you"); exit(666)`). Still invaluable — proving algorithms correct uncovers hidden
problems; theory and empirical testing are **equally important**.

## Related concepts

- [[partial-failure]] · [[byzantine-faults]] · [[truth-by-majority]] ·
  [[eventual-consistency]] (a liveness property)
- [[ch09-consistency-and-consensus]] — algorithms proved in these models

## Sources

DDIA Ch 8 ("System Model and Reality"). Ref: Dwork, Lynch, Stockmeyer, "Consensus in
the Presence of Partial Synchrony" (1988); Alpern & Schneider, "Defining Liveness"
(1985).
