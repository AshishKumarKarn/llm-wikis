---
title: Unreliable Networks
type: concept
chapters: [8]
tags: [distributed, networks, timeouts, congestion]
status: solid
updated: 2026-05-16
---

# Unreliable Networks

## Definition

Shared-nothing systems communicate only over an **asynchronous packet network**:
send a packet, **no guarantee** when/whether it arrives. A request with no response
is **fundamentally ambiguous** — request lost / node down / node paused / response
lost / response delayed are *indistinguishable*. *(DDIA Ch 8)*

## Network faults are common

Studies: ~12 faults/month in a medium datacenter; redundant gear doesn't help much
(human misconfig is a major cause); EC2 transient glitches; sharks bite cables;
asymmetric links (works one direction only). Even rare faults must be **handled**
(untested handling → deadlock or data deletion). Handling ≠ tolerating — showing an
error may suffice — but you must *know* the behavior and recover (Chaos Monkey).

## Detecting faults

Some feedback exists (RST/FIN on closed port, crash-notify scripts, switch mgmt
interface, ICMP unreachable) but **you can't count on it** — need a positive
application-level response. Otherwise: retry + **timeout**, then declare dead.

## Timeouts & unbounded delays

The only general detector is a timeout, but there's **no right value**: too long =
slow detection; too short = false positives → premature failover, duplicated actions,
load transfer → **cascading failure** (all nodes declare each other dead). A bounded
`2d + r` timeout would need bounded delay `d` + bounded handling `r` — real
asynchronous networks have **unbounded delays**. Choose timeouts *experimentally*, or
adaptively (Phi Accrual failure detector — Akka, Cassandra).

## Why delays are variable: queueing

Like traffic congestion: switch queues under network congestion (dropped → resent),
OS queue when CPUs busy, VM pauses (steal time), TCP flow control/backpressure +
retransmission. Worst near capacity; multi-tenant "noisy neighbors". TCP vs. UDP:
UDP drops flow control/retransmit — right when *delayed data is worthless* (VoIP).

## Related concepts

- [[partial-failure]] · [[synchronous-vs-asynchronous-networks]] (why we can't just
  make it reliable) · [[unreliable-clocks]] · [[process-pauses]]
- [[failover-and-split-brain]] · [[truth-by-majority]]

## Sources

DDIA Ch 8 ("Unreliable Networks").
