---
title: Horizontal Scaling
type: concept
tags: [scalability, partitioning]
sources: [consistent-hashing-primer]
created: 2026-05-15
updated: 2026-05-15
---

# Horizontal Scaling

Adding more nodes to share load, as opposed to vertical scaling (making one node bigger).
Vertical scaling hits a hard ceiling and a single point of failure; horizontal scaling is
the path to large-scale systems but introduces the problem of **distributing state across
nodes**.

## The core difficulty
For stateless services, horizontal scaling is easy — put a load balancer in front. For
**stateful** systems (caches, databases), you must decide which node owns which data. A
poor placement scheme makes scaling out actively harmful: with modulo hashing, adding one
node remaps almost all keys (see [[comparisons/modulo-vs-consistent-hashing]]).
[[concepts/consistent-hashing|Consistent hashing]] is the standard answer, bounding
relocation to ~1/N of keys per membership change (see [[sources/consistent-hashing-primer]]).

## Related
[[patterns/data-partitioning]] · [[components/distributed-cache]]
