---
title: "Synchronous vs. Asynchronous Networks (Circuit vs. Packet Switching)"
type: comparison
chapters: [8]
tags: [distributed, networks, comparison]
status: developing
updated: 2026-05-16
---

# Synchronous vs. Asynchronous Networks (Circuit vs. Packet Switching)

Why can't we just make the network reliable so software needn't worry? *(DDIA Ch 8)*

| | Telephone network (**circuit-switched**) | Internet/Ethernet (**packet-switched**) |
|---|---|---|
| Bandwidth | fixed, reserved per call (e.g. ISDN 16 bits/250 µs) | opportunistic, shared dynamically |
| Delay | **bounded** (no queueing — slot pre-reserved) | **unbounded** (queueing) |
| Idle cost | reserved bandwidth wasted | uses nothing when idle |
| Optimized for | constant-rate audio/video | **bursty** traffic (web, email, files) |

## The core trade-off

A circuit guarantees bounded delay but **wastes capacity** for bursty transfers (you
must guess a bandwidth allocation). Packet switching **maximizes utilization** (cheap
per byte) at the cost of queueing and variable delay. Hybrids exist (ATM, InfiniBand,
QoS + admission control) but QoS isn't enabled in multi-tenant clouds / the internet.

> **Variable network delay is not a law of nature — it's a cost/benefit trade-off**
> (dynamic resource partitioning: better utilization → cheaper, but variable delay).
> Same logic as CPU thread scheduling and VM multi-tenancy
> ([[response-time-percentiles]] / latency vs. utilization).

Consequence: no "correct" timeout value; assume congestion, queueing, unbounded
delays — see [[unreliable-networks]].

## Related

- [[unreliable-networks]] · [[partial-failure]] · [[system-models]] (synchronous /
  partially-synchronous / asynchronous)

## Sources

DDIA Ch 8 ("Synchronous Versus Asynchronous Networks").
