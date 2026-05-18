---
title: Kubernetes
type: component
tags: [kubernetes, orchestration, containers, devops]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Kubernetes (K8s)

Open-source container orchestration (Google, Go, 2014, CNCF). Automates deployment,
scaling, load balancing of containers. Core principle: a **reconciliation loop** drives
actual state toward declared **desired state** (YAML manifests) (see
[[sources/system-design-study-guide]]).

## Architecture
- **Cluster** = control plane + worker nodes. **Node** runs kubelet, container runtime,
  kube-proxy.
- **Control plane:** **API Server** (single entry point; only thing that talks to etcd) ·
  **etcd** (strongly consistent KV store, single source of truth) · **Scheduler** (places
  pods by resources/constraints/affinity) · **Controller Manager** (replica/node/endpoint
  controllers).
- **Pod creation flow:** kubectl → API Server validates → writes etcd → Scheduler picks
  node → API Server → kubelet creates pod + containers, streams status back to API Server
  → etcd.

## Workload objects
**Pod** (smallest unit, shared net/storage, ephemeral) · **ReplicaSet** (N identical
pods) · **Deployment** (manages ReplicaSets; rolling updates, rollbacks, self-healing,
zero-downtime) · **StatefulSet** (stable identity + storage; DBs, Kafka) · **DaemonSet**
(one pod per node; logging/monitoring agents) · **Job** (run to completion) · **CronJob**
(scheduled).

## Networking & config
**Service** = stable IP/DNS over ephemeral pods: ClusterIP (internal), NodePort (static
port per node, cluster-global), LoadBalancer (cloud LB via nodeport). **Ingress** +
Ingress Controller (NGINX/ALB): host/path routing, SSL termination. **kube-proxy**:
round-robin routing. **Network Policies**: namespace-scoped pod-level firewall
(default allow-all until restricted). **ConfigMap** (non-secret config; subPath mount
won't get updates) · **Secret** (base64, unencrypted in etcd by default — encrypt
separately). **Namespace** = isolation + resource quotas.

## Reliability & scaling
**Liveness probe** (fail ⇒ restart) vs **Readiness probe** (fail ⇒ remove from endpoints,
no restart). **Resource requests** (min guaranteed) vs **limits** (max, prevents
OOMKilled). **HPA** scales pods by CPU/memory/custom metrics. **PV** (cluster storage:
EBS/NFS) ↔ **PVC** (pod's request); StorageClass dynamically provisions; access modes
RWO/ROX/RWX; reclaim Delete vs Retain; `WaitForFirstConsumer` best for multi-zone.

## Related
[[components/load-balancer]] · [[components/aws-ec2]] (nodes) · [[concepts/horizontal-scaling]]
