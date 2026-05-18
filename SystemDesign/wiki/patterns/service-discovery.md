---
title: Service Discovery Pattern
type: pattern
tags: [service-discovery, microservices, registry, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Service Discovery Pattern

Tracks dynamically assigned network locations (IP:port) of microservice instances
so services can find each other without hardcoded addresses
(see [[sources/design-patterns-study-guide]]).

## Why it's needed
Microservices in containers/VMs get dynamic addresses. Without discovery, service
locations are hardcoded → tight coupling → brittle system. Service Discovery
decouples location from identity.

## Two parts
1. **Registration** — instance announces itself ("I'm here!") on startup.
2. **Lookup** — caller queries the registry to find available instances.

## Discovery models

### Client-Side Discovery
Consumer queries the Service Registry directly, picks an instance using a
load-balancing algorithm, and calls it.
- **Pro:** saves a network hop (no dedicated load balancer).
- **Con:** every client must implement LB logic in each language/framework.

### Server-Side Discovery
Client calls a load balancer; the LB queries the registry and routes the request.
- **Pro:** clients stay lightweight — no LB logic needed.
- **Con:** LB is an extra hop; needs to be highly available.

## Registration models

| Model | Who registers | Trade-offs |
|---|---|---|
| Self-registration | Service instance itself (sends heartbeats) | Simple; couples instance to registry |
| Third-party registration | Separate registrar polls/subscribes to deployment events | Decoupled; requires another component to manage |

## Service Registry
Central DB of instance addresses — must be **highly available** (cluster with
replication). Clients can cache entries but cache goes stale; registry handles
re-registration on expiry.

## Tools
Consul, Eureka (Netflix/Spring Cloud), etcd, ZooKeeper, Kubernetes DNS (built-in
server-side discovery via kube-proxy).

## Related
[[components/kubernetes]] (kube-proxy + CoreDNS = server-side discovery) ·
[[patterns/api-composition]] · [[patterns/feign-client]] (uses Eureka + Ribbon for discovery + LB)
