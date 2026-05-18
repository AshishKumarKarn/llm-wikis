---
title: Sloppy Quorums & Hinted Handoff
type: concept
chapters: [5]
tags: [replication, quorum, availability, dynamo]
status: developing
updated: 2026-05-16
---

# Sloppy Quorums & Hinted Handoff

## Definition

A network interruption can cut a client off from its n "home" nodes for a key while
other nodes are reachable. **Sloppy quorum**: still require w/r successful responses,
but allow them to come from reachable nodes *outside* the designated n.
**Hinted handoff**: once the network heals, those temporarily-accepted writes are
sent to the proper home nodes. *(DDIA Ch 5)*

> Analogy: locked out of your house, you sleep on a neighbor's couch (sloppy quorum);
> when you find your keys, you go home (hinted handoff).

## Trade-off

Increases **write availability** (any w nodes suffice), but it is **not a quorum in
the traditional sense**: even with w + r > n you may *not* read the latest value
until hinted handoff completes — it's only a durability assurance (data on w nodes
*somewhere*). Defaults: on in Riak, off in Cassandra/Voldemort.

## Related concepts

- [[quorum-consistency]] · [[leaderless-replication]] · [[eventual-consistency]]

## Sources

DDIA Ch 5 ("Sloppy Quorums and Hinted Handoff").
