---
title: 12 Factor App
type: concept
tags: [twelve-factor, cloud-native, microservices, deployment, concepts]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# 12 Factor App

A methodology for building cloud-native, portable, maintainable applications.
Each factor addresses a specific dimension of production-readiness
(see [[sources/design-patterns-study-guide]]).

## The 12 Factors

| # | Factor | Rule | Example |
|---|---|---|---|
| 1 | **Codebase** | One repo per app; tracked in VCS | One Git repo deployed to dev/stage/prod |
| 2 | **Dependencies** | Explicitly declare all dependencies | `package.json`, `pom.xml`; no system-installed libraries |
| 3 | **Config** | Store config in environment variables | `DB_URL` via env var, never hardcoded |
| 4 | **Backing services** | Treat external services as attached resources | Switch local DB → cloud DB via config only |
| 5 | **Build/release/run** | Strictly separate build and runtime stages | Build Docker image → inject config → run container |
| 6 | **Processes** | Run app as stateless processes | Sessions in Redis, not in-memory |
| 7 | **Port binding** | Expose services via a port | Spring Boot on port 8080; no runtime dependency |
| 8 | **Concurrency** | Scale out via process model | Run 5 containers instead of adding threads |
| 9 | **Disposability** | Fast startup, graceful shutdown | Handles SIGTERM cleanly; Kubernetes can safely terminate |
| 10 | **Dev/prod parity** | Keep environments similar | Same DB type across dev, staging, prod |
| 11 | **Logs** | Treat logs as event streams | Log to stdout; let ELK or CloudWatch collect |
| 12 | **Admin processes** | Run admin tasks as one-off processes | DB migration as a separate command |

## Key implications for microservices
- **Factor 3 (Config)** + **Factor 4 (Backing services):** services are environment-agnostic and deploy anywhere without code changes.
- **Factor 6 (Stateless processes):** enables horizontal scaling and disposability without sticky sessions.
- **Factor 9 (Disposability):** enables [[patterns/deployment-strategies#Rolling]] and autoscaling.
- **Factor 10 (Dev/prod parity):** reduces "works on my machine" production surprises.
- **Factor 11 (Logs):** pairs with [[components/elk-stack]] for centralized observability.

## Related
[[patterns/deployment-strategies]] · [[components/kubernetes]] · [[components/elk-stack]]
