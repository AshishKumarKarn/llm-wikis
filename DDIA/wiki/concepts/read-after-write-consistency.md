---
title: Read-After-Write Consistency
type: concept
chapters: [5]
tags: [consistency, replication]
status: developing
updated: 2026-05-16
---

# Read-After-Write Consistency

## Definition

(a.k.a. **read-your-writes**) A guarantee that a user **always sees updates they
submitted themselves**. No promise about *other* users' updates. *(DDIA Ch 5)*

## The anomaly it fixes

Under async [[replication-lag]]: user writes (to leader), then reads (from a stale
follower) and their own submission appears lost → user unhappy.

## Implementation techniques

- Read things the user *may* have modified **from the leader** (e.g. always read own
  profile from leader, others from followers).
- If most things are user-editable: for ~1 min after a user's last update, read from
  leader; or block follower reads if the follower lags > 1 min.
- Client remembers timestamp of its last write; serve reads only from a replica
  caught up to that timestamp (logical timestamp / log sequence number, or system
  clock → clock-sync critical, see [[ch08-the-trouble-with-distributed-systems]]).
- Multi-datacenter: route leader-needed requests to the leader's datacenter.
- **Cross-device**: timestamp metadata must be centralized; route a user's devices
  to the same datacenter.

## Related concepts

- [[replication-lag]] · [[eventual-consistency]] · [[monotonic-reads]] ·
  [[consistent-prefix-reads]]

## Sources

DDIA Ch 5 ("Reading Your Own Writes"). Ref: Terry et al., "Session Guarantees"
(1994).
