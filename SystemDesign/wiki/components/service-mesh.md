---
title: Service Mesh
type: component
tags: [service-mesh, istio, linkerd, sidecar, microservices, components]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Service Mesh

An infrastructure layer dedicated to managing service-to-service (east-west)
communication in microservices. Operates at the application layer via sidecar
proxies — developers focus on code, mesh handles networking
(see [[sources/design-patterns-study-guide]]).

## What it is
The [[patterns/sidecar-pattern]] applied at cluster scale. Every service gets an
injected sidecar proxy (Envoy). The proxies form a mesh; a control plane
configures them centrally.

```
Service A ←─ Envoy ──────────── Envoy ─→ Service B
                  ↑                  ↑
               Control Plane (Istio/Linkerd)
```

## What it manages

| Capability | Details |
|---|---|
| Traffic management | Routing rules, load balancing, retries, timeouts, circuit breaking |
| Security | Mutual TLS (mTLS) between every service pair — automatic, zero app changes |
| Observability | Distributed tracing, metrics, access logs per request |
| Deployment support | Canary deployments, traffic splitting by version/weight |
| Failure handling | Auto-retry, circuit breaking, rerouting to healthy instances |

## Why it matters (without a mesh)
As services grow from 5 to 500, managing retries, mTLS, tracing per-service
in application code is duplicated, inconsistent, and language-specific.
The mesh externalizes all of this.

## Popular tools

| Tool | Key strength |
|---|---|
| **Istio** | Most feature-rich; traffic mgmt, mTLS, canary, observability; complex |
| **Linkerd** | Lightweight, simpler ops, Kubernetes-native; uses Rust proxies |
| **Consul Connect** | Multi-platform (not just K8s); HashiCorp ecosystem |

Netflix uses Istio to route traffic by location, handle failures gracefully, secure
service comms with mTLS, and deploy new versions without user impact.

## Service Mesh vs API Gateway
| | API Gateway | Service Mesh |
|---|---|---|
| Traffic direction | North-south (external → internal) | East-west (service → service) |
| Location | Edge | Per-service sidecar |
| Focus | Auth, rate limiting, routing to entry | mTLS, retries, tracing, canary |

They are **complementary**: gateway at the edge, mesh inside the cluster.

## Trade-offs
- ❌ Operational complexity (control plane to manage)
- ❌ Extra latency per hop (two sidecar hops per call)
- ❌ Resource overhead (sidecar per pod)
- ✅ Consistent behavior across all services regardless of language

## Related
[[patterns/sidecar-pattern]] (the building block) · [[components/api-gateway]] ·
[[components/kubernetes]] · [[patterns/circuit-breaker]] (mesh can provide this) ·
[[patterns/deployment-strategies]] (canary deployments via traffic splitting)
