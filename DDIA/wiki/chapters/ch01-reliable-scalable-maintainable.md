---
title: "Ch 1 — Reliable, Scalable, and Maintainable Applications"
type: chapter
chapters: [1]
tags: [foundations, reliability, scalability, maintainability]
status: solid
updated: 2026-05-16
---

# Ch 1 — Reliable, Scalable, and Maintainable Applications

## One-paragraph thesis

Modern applications are **data-intensive** (bounded by data volume, complexity, and
rate of change) rather than compute-intensive. They are assembled from standard
building blocks — databases, caches, search indexes, stream processing, batch
processing — and the act of stitching these into a composite system makes you a *data
system designer*, not just an application developer. The chapter establishes the three
non-functional concerns that the rest of the book is organized around — **[[reliability]]**,
**[[scalability]]**, and **[[maintainability]]** — and gives precise vocabulary for
reasoning about each. *(DDIA Ch 1)*

## Key ideas

- The boundaries between data-system categories are blurring: datastores used as
  queues ([[redis]]), queues with database-like durability ([[apache-kafka]]). "One
  size fits all" is over — see [[one-size-fits-all-paper]].
- **[[reliability]]** = continuing to work correctly even when things go wrong. The
  pivotal distinction is **[[fault-tolerance|fault vs. failure]]**: a fault is a
  component deviating from spec; a failure is the system as a whole stopping. You
  build reliable systems from unreliable parts by stopping faults from becoming
  failures.
- Faults come in three flavors: **hardware** (mostly random, uncorrelated),
  **software** (systematic, correlated across nodes, dormant until triggered), and
  **human** (the leading cause of outages — config errors).
- **[[scalability]]** is not a label ("X scales") but a question: *if load grows in a
  particular way, what are our options for coping?* Requires quantifying load via
  **[[load-parameters]]** and performance via **[[response-time-percentiles]]**.
- The Twitter timeline case study: fan-out-on-write vs. fan-out-on-read, and the
  hybrid that handles celebrity skew — the canonical [[load-parameters]] example.
- **[[maintainability]]** decomposes into **operability**, **simplicity** (fighting
  *accidental* complexity via good abstractions), and **evolvability**.

## Concepts introduced

- [[reliability]]
- [[fault-tolerance]]
- [[scalability]]
- [[load-parameters]]
- [[response-time-percentiles]]
- [[shared-nothing-architecture]]
- [[maintainability]]

## Systems / papers referenced

- [[apache-kafka]], [[redis]] — category-blurring examples
- Netflix Chaos Monkey — deliberate fault injection (see [[fault-tolerance]])
- Hadoop — throughput-oriented batch system (revisited [[ch10-batch-processing]])
- [[the-tail-at-scale]] — Dean & Barroso, tail latency amplification
- [[one-size-fits-all-paper]] — Stonebraker & Çetintemel

## Trade-offs & tensions

- **Prevent vs. tolerate faults.** Generally prefer tolerating; prevention wins only
  where no cure exists (security: a leaked secret cannot be un-leaked).
- **Reliability vs. cost.** Acceptable to consciously cut reliability for prototypes
  or thin-margin services — the rule is *be conscious of when you cut corners*.
- **Scale up vs. scale out.** See [[shared-nothing-architecture]]. Stateless scaling
  is easy; stateful (database) scaling adds large complexity — old wisdom was "scale
  up until forced otherwise."
- **Restrictive abstractions backfire.** Error-minimizing interfaces, if too rigid,
  get worked around — negating the benefit.
- **Optimizing percentiles has diminishing returns.** Amazon targets p99.9 but
  judged p99.99 too costly for the benefit.

## Connections to other chapters

- Rolling upgrades (tolerating machine loss without downtime) → [[ch04-encoding-and-evolution]].
- Twitter hybrid timeline revisited → [[ch12-the-future-of-data-systems]].
- Rebalancing / elastic scaling → [[ch06-partitioning]].
- Composite multi-component systems (Figure 1-1) → Part III derived data.

## Open questions / things to revisit

- How does the fan-out write-vs-read trade-off generalize to [[ch11-stream-processing]] materialized views?
- Where does the book land on "distributed by default" becoming the norm?
