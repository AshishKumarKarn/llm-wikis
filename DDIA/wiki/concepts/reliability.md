---
title: Reliability
type: concept
chapters: [1]
tags: [reliability, foundations, fault-tolerance]
status: solid
updated: 2026-05-16
---

# Reliability

## Definition

Reliability is a system **continuing to work correctly even when things go wrong** —
performing the expected function at the desired performance, tolerating user
mistakes, and preventing unauthorized access, under expected load. *(DDIA Ch 1)*

## Why it matters / what problem it solves

"Working correctly even when things go wrong" is the baseline contract with users.
The book stresses this is *not* only for nuclear/aviation software: corrupted photo
databases, lost productivity, and ecommerce outages all carry real human and revenue
cost. Reliability can be *consciously* traded away for prototypes or thin-margin
services — the discipline is being aware when you cut the corner.

## How it works (the mechanism)

The core move is the **[[fault-tolerance|fault → failure]]** distinction: design
mechanisms that stop a *fault* (one component off-spec) from becoming a *failure*
(the whole system failing the user). You cannot drive fault probability to zero, so
you build **reliable systems from unreliable parts**. Three fault classes:

- **Hardware faults** — disk MTTF ~10–50 yrs ⇒ a 10,000-disk cluster loses ~1/day.
  Mitigation: redundancy (RAID, dual PSU, generators) plus, increasingly,
  software fault-tolerance that tolerates whole-machine loss (enables rolling
  upgrades — see [[ch04-encoding-and-evolution]]). Mostly **random, uncorrelated**.
- **Software errors** — systematic bugs, **correlated across nodes** (e.g. the 2012
  leap-second Linux hang), runaway resource use, cascading failures. Dormant until
  an environmental assumption breaks. No quick fix: testing, process isolation,
  crash-restart, measurement, self-checks on invariants.
- **Human errors** — config errors by operators are the *leading* cause of outages
  (hardware only 10–25%). Mitigate via good abstractions/APIs, sandboxes,
  thorough testing, fast rollback/gradual rollout, telemetry, training.

Counterintuitively, deliberately **injecting faults** (Netflix Chaos Monkey)
continually exercises the fault-tolerance machinery so it works when faults occur
naturally — see [[fault-tolerance]].

## Trade-offs

- **Prevent vs. tolerate.** Prefer tolerance; prevent only where no cure exists
  (security breaches are irreversible).
- **Reliability vs. development/operational cost** — a conscious decision, not a
  default.

## Real systems that use it

Pattern is universal; specific techniques appear throughout — replication
([[ch05-replication]]), partitioning ([[ch06-partitioning]]), transactions
([[ch07-transactions]]), consensus ([[ch09-consistency-and-consensus]]).

## Related concepts

- [[fault-tolerance]] — the central mechanism
- [[scalability]], [[maintainability]] — the other two pillars
- [[response-time-percentiles]] — "desired level of performance" is measured here

## Sources

DDIA Ch 1 ("Reliability"). Related: [[the-tail-at-scale]].
