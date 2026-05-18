---
title: "Modulo Hashing vs. Consistent Hashing"
type: comparison
tags: [hashing, sharding, scalability]
sources: [consistent-hashing-primer]
created: 2026-05-15
updated: 2026-05-15
---

# Modulo Hashing vs. Consistent Hashing

Both answer "which node owns this key?" for a [[patterns/data-partitioning|partitioned]]
system. They differ catastrophically in behavior when the node count changes.

| Dimension | Modulo: `hash(key) % N` | [[concepts/consistent-hashing|Consistent hashing]] |
|---|---|---|
| Keys moved when N changes | ~all keys (`%4` vs `%5` agree on almost nothing) | ~K/N keys (only the affected arc) |
| Resize behavior | Cache stampede → DB overload | Most keys stay put / warm |
| Load balance | Even (when N fixed) | Even **only with virtual nodes** |
| Capacity weighting | Not supported | Yes, via token count per node |
| Implementation cost | Trivial | Ring + virtual nodes (moderate) |
| When acceptable | N truly fixed forever | Any system that adds/removes nodes |

## Verdict
Modulo hashing is fine only if the node set never changes — rare in real systems. The
moment you scale a [[components/distributed-cache|distributed cache]] or sharded store,
modulo's full-remap behavior makes it a liability, and consistent hashing (with virtual
nodes) is the standard choice (see [[sources/consistent-hashing-primer]]).
