---
title: Sidecar Pattern
type: pattern
tags: [sidecar, service-mesh, infrastructure, microservices, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Sidecar Pattern

A helper component runs alongside a service, sharing the same lifecycle, and handles
cross-cutting infrastructure concerns without modifying application code
(see [[sources/design-patterns-study-guide]]).

## Core idea
Think of the sidecar as an assistant container/process. The app does business logic;
the sidecar handles networking, observability, security, config.

```
Pod
  ├── App Container    ← business logic
  └── Sidecar Container ← infrastructure logic

App communicates with sidecar via: local network, shared filesystem, or IPC.
```

## What it handles (without code changes to the app)
- Traffic routing, retries, timeouts
- mTLS / encryption
- Observability (metrics, tracing, access logs)
- Log shipping (stdout → ELK/Splunk)
- Secrets / config fetching (Vault → local file → app reads)

## Real-world examples
| Example | Sidecar | What it does |
|---|---|---|
| Service Mesh | Envoy proxy (Istio/Linkerd) | Traffic mgmt, mTLS, observability |
| Log shipping | Filebeat / Fluentd | Collect app logs, ship to ELK |
| Secrets | Vault Agent Injector | Fetch secrets, expose to app |

## Sidecar vs Library approach
| Aspect | Sidecar | Library |
|---|---|---|
| Language support | Language-agnostic | Language-specific |
| Lifecycle | Independent | Tied to app |
| Updates | Central; no app rebuild | App must be rebuilt |

Sidecars win when you have many services in different languages.

## Trade-offs
- ❌ Extra resource usage (CPU/memory per pod)
- ❌ Increased operational complexity
- ❌ More moving parts; adds latency per hop

## Foundation of Service Mesh
The [[components/service-mesh]] architecture is the Sidecar Pattern applied at
cluster scale — every service gets an injected proxy (Envoy), forming a mesh.

## Related
[[components/service-mesh]] · [[components/kubernetes]] (Pod hosts both containers) ·
[[components/api-gateway]] (gateway = north-south; sidecar = east-west)
