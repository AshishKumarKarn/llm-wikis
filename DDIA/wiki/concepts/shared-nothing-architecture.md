---
title: Shared-Nothing Architecture (Scale Up vs. Scale Out)
type: concept
chapters: [1]
tags: [scalability, architecture, distributed]
status: developing
updated: 2026-05-16
---

# Shared-Nothing Architecture (Scale Up vs. Scale Out)

## Definition

- **Scaling up (vertical):** move to a more powerful machine.
- **Scaling out (horizontal):** distribute load across many smaller machines.
- **Shared-nothing architecture:** the horizontal approach where nodes share no
  memory or disk and coordinate over the network. *(DDIA Ch 1)*

## Why it matters

It is the primary lever for *coping with load* once [[load-parameters]] and
[[response-time-percentiles|performance]] are quantified. High-end single machines
get very expensive, so intensive workloads can't avoid scaling out — but good
architectures are a *pragmatic mixture* (a few powerful machines can beat a swarm of
tiny VMs in simplicity and cost).

## How it works (the mechanism)

- **Stateless services** distribute across machines easily.
- **Stateful data systems** are much harder to take from one node to distributed —
  this is the complexity that motivates most of the book (Part II). Old common
  wisdom: keep the database on a single node (scale up) until cost or
  high-availability requirements force distribution. As distributed-systems tooling
  improves, distributed-by-default may become the norm.
- **Elastic vs. manual:** elastic systems auto-add resources on detected load
  increase (good for unpredictable load); manually scaled systems are simpler with
  fewer operational surprises. Rebalancing detail → [[ch06-partitioning]].

## Trade-offs

- Single-node simplicity vs. distributed scalability/availability.
- Elastic flexibility vs. manual predictability.
- Premature scale-out adds distributed-systems complexity for load that may never
  arrive.

## Related concepts

- [[scalability]] — parent concept
- [[shared-memory-vs-shared-disk-vs-shared-nothing]] — the Part II three-way view
- [[replication-vs-partitioning]] — the two distribution mechanisms
- [[ch05-replication]], [[ch06-partitioning]] — the two core ways stateful systems
  go distributed
- [[ch08-the-trouble-with-distributed-systems]] — the cost of "shares nothing,
  coordinates over an unreliable network"

## Sources

DDIA Ch 1 ("Approaches for Coping with Load").
