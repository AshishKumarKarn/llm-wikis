---
title: EM — Technical Leadership
type: behavioral
tags: [leadership, architecture, tech-debt, production, observability, behavioral]
sources: [leadership]
created: 2026-05-16
updated: 2026-05-16
---

# EM — Technical Leadership

Interview answers for architecture, production, observability, tech debt, and
SA/EM technical decision-making (see [[sources/leadership-study-guide]]).

## Architecture Decisions

**Q: How do you handle disagreements on architecture?**
"I encourage design docs and review sessions. Evaluate options using data, benchmarks,
and proof of concept. If disagreement persists, timebox the discussion and escalate with
options + trade-offs. Ego should never drive architecture."

**Q: How do you prevent architectural sprawl in large teams?**
- Architecture review board.
- Shared design standards and API governance.
- Internal tech wiki (a [[components/elk-stack]] for knowledge).
- Reusable platform components.

**Q: How do you evaluate new technology adoption?**
4-step framework:
1. **Problem clarity** — is there a real problem this solves?
2. **Maturity assessment** — is it production-ready?
3. **Proof of concept** — validate in a non-critical service.
4. **Migration cost analysis** — what's the switching cost?
"I avoid adopting tech just because it's trending."

**Q: How do you decide monolith vs microservices?**
"If team size is small and domain boundaries are unclear → prefer modular monolith.
Move to microservices when: independent scaling is required, clear domain boundaries
exist ([[concepts/domain-driven-design]]), deployment independence becomes critical.
Premature microservices often create operational overhead."

**Q: How do you design for multi-region deployment?**
- Active-active or active-passive setup.
- Global load balancing + latency-based routing.
- Regional failover strategy.
- Data replication model defined clearly (RPO and RTO explicit).

**Q: How do you create a long-term technical vision?**
"Align with 3-year business strategy. Break into: Foundation → Scale → Optimization phases.
Communicate vision repeatedly so teams build toward it incrementally."

## Tech Debt

**Q: How do you handle technical debt?**
Categorize into:
1. **Risk debt** — active reliability/security risk. Address immediately.
2. **Velocity debt** — slows feature delivery. Prioritize in quarterly planning.
3. **Cosmetic debt** — readability/structure. Low priority.

"We allocate 15–20% of sprint capacity toward debt reduction. Architecture reviews quarterly
to avoid silent accumulation."

## Production & Reliability

**Q: How do you handle a production outage?**
1. **Stabilize** system first (rollback, circuit breaker, rate limiting).
2. **Communicate** clearly and frequently to stakeholders.
3. **Assign** clear roles (incident commander, comms, diagnostics).
4. **Blameless postmortem** — focus on systemic fixes.
5. **Implement** preventive measures (monitoring, chaos testing, runbooks).

**Q: How do you ensure system reliability?**
- Load balancers + health checks.
- Graceful degradation.
- Database replication + automated failover.
- Backup and restore testing.
- Game days to test failure handling.

**Q: How do you balance speed vs quality?**
"Define what must be perfect vs what can iterate:
- Core financial flows → strong quality gates.
- UI experiments → faster iteration with monitoring."

## Observability

**Q: How do you design for observability?**
- Structured logging (SLF4J/Logback — never `System.out.println`).
- Metrics with SLIs/SLOs.
- [[concepts/distributed-tracing]] (Jaeger/Zipkin with trace IDs through all services).
- Alerting based on symptoms, not causes.
- "If we can't observe it, we can't scale it."

**Q: How do you ensure architecture aligns with business goals?**
"Anchor architecture to business KPIs:
- If retention matters → performance & UX optimized.
- If enterprise clients matter → security & audit logs prioritized.
Participate in quarterly planning to align tech roadmap with revenue goals."

## Communicating Architecture to Non-Technical Stakeholders

Instead of: "We implemented distributed caching."
Say: "We added a caching layer to reduce response times and support higher user traffic."

Focus on **business impact**, not implementation. Avoid deep technical jargon.

## SA/EM Interview Question Themes

| Question type | What they evaluate | Strong answer includes |
|---|---|---|
| System design | Structured thinking, NFRs | Business requirements, scalability, delivery roadmap |
| Stakeholder alignment | Communication, risk management | MVP first, continuous feedback loop |
| Architecture trade-off | Decision-making | Problem → Options → Trade-offs → Decision → Outcome |
| Under-resourced delivery | Pragmatic thinking | MVP approach, prioritize critical features, tech debt management |
| Team conflict | Conflict resolution | Listen, data-driven, facilitate consensus, decide if needed |
| Architecture governance | Leadership | Principles, reviews, standards, documentation |
| Production scalability | Production mindset | Capacity planning, load testing, monitoring, failover |

## Related
[[behavioral/em-people-management]] · [[behavioral/sdlc-and-delivery]] ·
[[scenarios/production-incident-response]] · [[concepts/domain-driven-design]]
