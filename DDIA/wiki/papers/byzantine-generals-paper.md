---
title: "The Byzantine Generals Problem (Lamport, Shostak & Pease, 1982)"
type: paper
chapters: [8]
tags: [paper, distributed, byzantine, consensus]
status: stub
updated: 2026-05-16
---

# The Byzantine Generals Problem

Leslie Lamport, Robert Shostak, Marshall Pease, *ACM TOPLAS* 4(3):382–401, July 1982.
doi:10.1145/357172.357176

Defines the canonical problem behind [[byzantine-faults]]: n generals must agree on a
plan despite **traitors** sending fake/contradictory messages; identities of traitors
unknown. Generalizes the **Two Generals Problem**. Foundational for Byzantine
fault-tolerant consensus (relevant to aerospace, blockchains; usually out of scope
for trusted datacenters).

> `status: stub` — anchor for [[byzantine-faults]]; consensus → Ch 9.

## Related

- [[byzantine-faults]] · [[ch09-consistency-and-consensus]] ·
  [[lamport-clocks-paper]]
