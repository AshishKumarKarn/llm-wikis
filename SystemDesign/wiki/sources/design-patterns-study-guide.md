---
title: "Source: Design Patterns Study Guide"
type: source
tags: [microservices, patterns, gof, solid, deployment]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Source: Design Patterns Study Guide

**Origin:** `raw/Design Patterns.pdf` · 101 pages · personal study notes (OneNote export),
Jan–Mar 2026 · interview prep (EM / SA focus).

## Summary
Two sections: (1) Microservices/distributed patterns — rich text, p1–71; (2) GoF design
patterns + SOLID — image-only pages, no extractable text (p72–101).

## Section index → wiki pages
- Saga (p1–2) → [[patterns/saga]]
- Bulkhead (p3–6) → [[patterns/bulkhead]]
- Circuit Breaker (p7–11) → [[patterns/circuit-breaker]]
- CQRS (p12–13) → [[patterns/cqrs]]
- Service Discovery (p14–18) → [[patterns/service-discovery]]
- Retry Pattern (p19–21) → [[patterns/retry-pattern]]
- Outbox Pattern (p22–24) → [[patterns/outbox-pattern]]
- API Gateway (p25–27) → [[components/api-gateway]]
- Event Sourcing (p28–30) → [[patterns/event-sourcing]]
- Strangler Fig (p31–33) → [[patterns/strangler-fig]]
- Database Per Service (p34–36) → [[patterns/database-per-service]]
- Sidecar Pattern (p37–40) → [[patterns/sidecar-pattern]]
- API Composition (p41–43) → [[patterns/api-composition]]
- Feign Client (p44–45) → [[patterns/feign-client]]
- Dual Write Problem (p46–50) → [[patterns/dual-write-problem]]
- 12 Factor App (p51–52) → [[concepts/twelve-factor-app]]
- Deployment Strategies (p53–66) → [[patterns/deployment-strategies]]
- Service Mesh (p67–71) → [[components/service-mesh]]
- SOLID Principles (p72–77) → [[concepts/solid-principles]] *(image-only)*
- GoF Patterns (p78–101) → [[concepts/gof-patterns-overview]] *(image-only)*

## Key takeaways
1. The Outbox Pattern (+ CDC) is the canonical solution to the Dual Write Problem.
2. Database Per Service enforces true service autonomy; combined with Saga + Outbox for
   consistency.
3. Deployment strategies form a risk ladder: Recreate < Rolling < Blue-Green < Canary <
   Feature Flags. Modern systems combine them.
4. Service Mesh (Istio/Linkerd) = Sidecar Pattern applied to the whole cluster.

## Gaps
SOLID + GoF design patterns pages (p72–101) are image-only — wiki pages written from
general knowledge, not from extracted source text.
