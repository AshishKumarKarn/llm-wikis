---
title: Bulkhead Pattern
type: pattern
tags: [resilience, microservices, isolation, fault-tolerance, patterns]
sources: [system-design, design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Bulkhead Pattern

Named after ship watertight compartments: isolate resources/components so a failure or
overload in one area cannot cascade to others (see [[sources/design-patterns-study-guide]]).

## Types of bulkheads
| Type | How | Example |
|---|---|---|
| Thread pool | Separate pools per task type | User-facing requests ≠ background processing |
| Service | Each microservice independent resources/dependencies | Prevents cross-service cascade |
| Database | Separate DB instances per workload | Read-heavy vs write-heavy partitioned |
| Network | Dedicated paths per traffic type | High-priority vs low-priority lanes |
| Process/Container | Separate runtimes per component | K8s pods per tier |
| Resource | CPU/memory quotas per task | Prevents one task starving others |

## Challenges
Complexity of managing isolated pools · resource management overhead · synchronization
across bulkheads · added latency from isolation layers · scaling elasticity.

## In the design problems
[[scenarios/webhook-delivery-backoff]]: per-customer queue shards with bounded concurrency
caps (standard: 50, enterprise: 500, trial: 5). One slow customer can't block others.

## Related
[[patterns/circuit-breaker]] (complementary — CB stops calls; bulkhead isolates capacity) ·
[[scenarios/webhook-delivery-backoff]] · [[components/kubernetes]] (resource requests/limits)
