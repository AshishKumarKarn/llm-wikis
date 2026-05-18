---
title: Fencing Tokens
type: concept
chapters: [8]
tags: [distributed, locking, safety]
status: solid
updated: 2026-05-16
---

# Fencing Tokens

## Definition

A monotonically increasing number returned by the lock/lease service every time a
lock is granted. Every write to the protected resource must include the client's
current token; the **resource rejects any write with a token ≤ one it has already
processed**. *(DDIA Ch 8)*

## Why it's needed

[[process-pauses|Process pauses]] can make a client believe it still holds an expired
lease while another client has acquired a new one → both write → corruption
([[truth-by-majority|the leader-and-lock problem]]). Fencing stops a paused-and-
revived "chosen one" from disrupting the system.

## How it works

Client 1 gets token 33, pauses, lease expires. Client 2 gets token 34, writes
(token 34 recorded). Client 1 revives and writes with token 33 → **rejected** (34 >
33). ZooKeeper's `zxid` or node `cversion` work as fencing tokens (monotonic). Key
point: **the resource itself must check** — clients checking their own lock status is
insufficient ("it is unwise for a service to assume its clients are well behaved"). A
good thing: services should protect themselves from buggy/abusive clients.

## Limit

Detects/blocks *inadvertent* error, not a malicious node that forges a token →
that's a **[[byzantine-faults|Byzantine fault]]**.

## Related concepts

- [[truth-by-majority]] · [[process-pauses]] · [[byzantine-faults]] ·
  [[failover-and-split-brain]] · [[zookeeper]]
- [[ch09-consistency-and-consensus]] (where fencing tokens come from)

## Sources

DDIA Ch 8 ("Fencing Tokens").
