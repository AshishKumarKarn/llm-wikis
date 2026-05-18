---
title: Production Incident Response
type: scenario
tags: [incident, debugging, production, postmortem, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Production Incident Response

Structured approach to handling bugs and outages in production. Goal: contain impact
first, then diagnose, then fix safely. Mnemonic: **C-D-F-P**
(Contain → Diagnose → Fix → Prevent)
(see [[sources/scenario-questions-study-guide]]).

## Step 1: Immediate Containment
- **Triage:** identify severity (full outage vs degraded performance vs silent failure).
- **Rollback:** if a recent deployment caused the bug, roll back via deployment tooling.
- **Feature flag:** disable the problematic feature without redeploying.
- **Rate limiting / throttling:** protect downstream services from overload.

## Step 2: Rapid Diagnosis
- **Logs & Metrics:** ELK/Loki + Prometheus/Grafana — look for error rate spikes.
- **Distributed tracing:** Jaeger/Zipkin to pinpoint which microservice is failing.
- **Heap/thread dumps:** for JVM-based apps, capture to analyze resource bottlenecks.
- **Compare baselines:** traffic patterns before and after incident.
- **Check dependencies:** many bugs are DB latency, external API failures, or
  misconfigured caches — not application bugs.

## Step 3: Root Cause Analysis
- Reproduce in staging with mirrored traffic (shadow deployment).
- Configuration drift: verify env vars, secrets, and deployment configs.
- Silent failures: date parsing errors, concurrency issues, OOM — look at GC logs.

## Step 4: Safe Fix & Deployment
- Fix in a branch; run automated tests.
- Deploy via **canary release** (route 5% traffic first; monitor; expand).
- Validate P99 latency and error rates post-deployment before scaling to all nodes.

## Step 5: Postmortem & Prevention
- **Blameless postmortem:** focus on process improvements, not individuals.
- Add regression tests to prevent recurrence.
- Improve observability: add alerts for early detection.
- **Chaos testing:** simulate failures to validate resilience.
- Configuration hygiene: automate config validation to avoid drift.

## Quick decision matrix
| Situation | Immediate action | Longer-term fix |
|---|---|---|
| Deployment bug | Rollback / disable feature | Fix code, add tests |
| Traffic spike | Rate limiting, autoscaling | Capacity planning |
| Silent failure | Enable detailed logging | Add monitoring + validation |
| External dependency down | Circuit breaker, fallback | Retry/backoff strategy |

## Cascading failure isolation
For intermittent microservice failures:
1. **Detect:** dashboards (CPU, memory, latency, error rate).
2. **Isolate:** circuit breaker (stops routing to failing service); rate limiting.
3. **Fallback:** return cached data or graceful degraded response.
4. **RCA:** check DB latency, external API, network timeouts.
5. **Fix:** bulkhead isolation, timeouts with exponential backoff.

## Manager/SA-level framing
"I lead with contain → communicate to stakeholders → diagnose with data, not
guesses → fix via canary. Postmortem is blameless and focused on systemic fixes.
Every incident teaches us what to monitor better next time."

## Related
[[concepts/distributed-tracing]] · [[patterns/circuit-breaker]] · [[patterns/bulkhead]] ·
[[patterns/retry-pattern]] · [[patterns/deployment-strategies]] ·
[[concepts/jvm-debugging]]
