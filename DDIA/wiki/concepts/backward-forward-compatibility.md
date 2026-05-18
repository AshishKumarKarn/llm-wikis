---
title: Backward & Forward Compatibility
type: concept
chapters: [4]
tags: [encoding, evolvability, rolling-upgrade, compatibility]
status: solid
updated: 2026-05-16
---

# Backward & Forward Compatibility

## Definition

- **Backward compatibility**: newer code can read data written by **older** code.
- **Forward compatibility**: older code can read data written by **newer** code.
*(DDIA Ch 4)*

## Why it matters

Code changes aren't instantaneous. Server-side: **rolling upgrade / staged rollout**
(deploy to a few nodes at a time → no downtime, frequent low-risk releases — a direct
enabler of [[maintainability|evolvability]]). Client-side: users delay updates. So
**old/new code and old/new data formats coexist**, and the system must tolerate both
directions at once.

## The asymmetry

- **Backward compat is usually easy**: as author of the new code you know the old
  format and can explicitly handle it (or keep old read code).
- **Forward compat is trickier**: old code must *ignore* additions it doesn't
  understand without corrupting them. Hard part of [[schema-evolution]].

## Where it bites

Every mode of [[modes-of-dataflow|dataflow]]: databases (forward compat needed
because newer code may write a value an older still-running instance then reads;
"data outlives code"; preserve unknown fields), services/[[rpc-vs-rest|RPC]] (assume
servers upgrade before clients → need backward compat on requests, forward on
responses), and [[message-passing-dataflow|message passing]].

## Related concepts

- [[schema-evolution]] · [[data-encoding-formats]] · [[modes-of-dataflow]]
- [[maintainability]] — evolvability, the Ch 1 origin
- [[schema-on-read-vs-schema-on-write]] — the related Ch 2 axis

## Sources

DDIA Ch 4 (intro & Summary).
