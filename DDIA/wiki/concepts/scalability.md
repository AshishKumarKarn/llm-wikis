---
title: Scalability
type: concept
chapters: [1]
tags: [scalability, foundations, performance]
status: solid
updated: 2026-05-16
---

# Scalability

## Definition

Scalability is a system's ability to cope with increased load. It is **not a
one-dimensional label** — "X is scalable" is meaningless. It is the question: *if the
system grows in a particular way, what are our options for coping with that growth?*
*(DDIA Ch 1)*

## Why it matters

A system reliable today can degrade tomorrow under growth (10k → 100k users, larger
data). Scalability reasoning requires first quantifying **[[load-parameters]]** and
then **[[response-time-percentiles|performance]]**, because the right architecture
is entirely load-dependent: 100k req/s × 1 kB looks nothing like 3 req/min × 2 GB
even at equal throughput. There is no *generic* scalable architecture ("no magic
scaling sauce") — scalable architectures are built from general-purpose building
blocks arranged around assumptions about which operations are common vs. rare.

## How it works (the mechanism)

1. **Describe load** with [[load-parameters]] (req/s, read/write ratio, fan-out,
   cache hit rate — whatever dominates *your* bottleneck).
2. **Describe performance** — throughput (batch systems like Hadoop) or response
   time as a *distribution* measured via [[response-time-percentiles]], not a mean.
3. **Cope with load** via [[shared-nothing-architecture]] (scale up vs. out),
   elastic vs. manual scaling.

The Twitter timeline case study (fan-out-on-write vs. -on-read, plus the celebrity
hybrid) is the canonical worked example — detailed in [[load-parameters]].

## Trade-offs

- **Scale up vs. scale out** — see [[shared-nothing-architecture]].
- **Elastic vs. manually scaled** — elastic helps under unpredictable load; manual
  is simpler with fewer operational surprises.
- **Premature scaling.** Architectures encode load assumptions; if wrong, scaling
  effort is wasted or counterproductive. Early-stage products usually benefit more
  from fast product iteration than from scaling to hypothetical future load.

## Related concepts

- [[load-parameters]] — how to describe load (Twitter example)
- [[response-time-percentiles]] — how to measure performance
- [[shared-nothing-architecture]] — how to cope with load
- [[reliability]], [[maintainability]] — the other two pillars

## Sources

DDIA Ch 1 ("Scalability"). Twitter data: Krikorian, "Timelines at Scale" (QCon 2012).
