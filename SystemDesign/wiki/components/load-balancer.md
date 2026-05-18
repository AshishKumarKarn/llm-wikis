---
title: Load Balancer
type: component
tags: [load-balancing, scalability, availability]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Load Balancer

Distributes incoming requests across computing resources (app servers, databases) and
returns responses to clients. Prevents routing to unhealthy servers, prevents overload,
eliminates single points of failure. Hardware (expensive) or software (HAProxy, NGINX).
Enables [[concepts/horizontal-scaling]] (see [[sources/system-design-study-guide]]).

## L4 vs L7
- **Layer 4 (transport):** routes on source/dest IP + port, no payload inspection; does
  NAT; cheap and fast.
- **Layer 7 (application):** inspects headers/body/cookies; terminates traffic, decides,
  reconnects upstream. Enables content routing (video → media servers, billing →
  hardened servers). More flexible, more costly.

## Algorithms
- **Static:** random; round robin; weighted round robin (by capacity); sticky/session
  round robin (session consistency).
- **Dynamic:** least connections; lowest response time (often combined with least conn).
- **Hash-based:** IP hashing (session stickiness via client IP); URL hashing (route by
  path for data locality). Hash-based routing relates to [[concepts/consistent-hashing]].
- No single algorithm fits all; hybrids switch strategies under peak load.

## Additional capabilities
SSL termination (offload decrypt/encrypt; no per-server certs) · session persistence via
cookies · health checks reroute away from unhealthy servers.

## Availability & caveats
Deploy multiple LBs active-active or active-passive — a single LB is itself a SPOF, and
multiple LBs add complexity. An under-resourced LB becomes the bottleneck. Horizontal
scaling behind an LB requires **stateless** servers (sessions in Redis/DB) and downstream
caches/DBs that can absorb more connections as the tier scales out.

## Related
[[concepts/horizontal-scaling]] · [[components/kubernetes]] (Service/Ingress) ·
[[components/caching]]
