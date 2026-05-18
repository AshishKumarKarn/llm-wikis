---
title: Deployment Strategies
type: pattern
tags: [deployment, ci-cd, zero-downtime, canary, blue-green, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Deployment Strategies

A risk ladder from simple-but-disruptive to complex-but-safe. Modern systems
combine strategies (see [[sources/design-patterns-study-guide]]).

## Strategy overview

### 1. Recreate
Stop all old instances → deploy new version → start new instances. Simple.
- **Pro:** no version compatibility issues; cheapest infra.
- **Con:** downtime is unavoidable.
- **Use when:** internal tools, low-traffic, batch systems; schema-breaking DB changes.

### 2. Rolling
Replace instances in batches (e.g., 2 at a time). Default in Kubernetes
(`maxUnavailable`, `maxSurge`).
- **Pro:** no full downtime; resource-efficient.
- **Con:** old + new coexist → backward compatibility required; rollback takes time.
- **Use when:** standard web apps, microservices, moderate-risk changes.

### 3. Blue-Green
Two identical environments: **Blue** = live, **Green** = new version.
Deploy and test Green, then switch load balancer. Keep Blue as rollback.
- **Pro:** zero downtime; **instant** rollback.
- **Con:** double infrastructure cost; DB schema must be backward compatible.
- **Use when:** high-availability, customer-facing, enterprise SaaS.
- **Interview:** "For critical services, I prefer blue-green because rollback is immediate."

### 4. Canary
Route a small % of traffic (e.g., 5%) to the new version. Monitor metrics
(error rate, latency, CPU). Gradually increase if stable.
- **Pro:** real-world validation; minimizes blast radius; data-driven confidence.
- **Con:** requires strong monitoring; more complex routing.
- **Use when:** risky features, performance-sensitive systems, ML model rollouts.

### 5. A/B Testing
Different user groups see different versions — used to measure **business** impact
(conversion rates, UX). Not for stability testing (that's Canary).
- **Use when:** UI/UX changes, pricing experiments, recommendation systems.

### 6. Shadow (Dark Launch)
Real production traffic is **mirrored** to the new system; new system processes
requests but its responses are discarded. Users still see old system responses.
- **Pro:** test with real load; zero user risk.
- **Con:** extra infrastructure; complex observability.
- **Use when:** performance-testing infra rewrites, DB engine migrations.

### 7. Feature Flags
Code is deployed but functionality is hidden behind a config flag.
Turn on for internal users, beta users, or % rollout.
- **Pro:** instant rollback (flip the flag); decouples deployment from release.
- **Con:** flag debt; increased code complexity; needs governance and cleanup.
- **Use when:** continuous delivery pipelines, most modern SaaS.

## Risk ladder
```
Recreate < Rolling < Blue-Green < Canary < Feature Flags
(most risky)                            (safest)
```

## How to choose (interview framing)
1. Assess risk level of the change.
2. Evaluate user impact tolerance.
3. Check infrastructure capability.
4. Decide rollback requirement.

"For a high-risk change in a high-availability system, I'd use canary with automated
rollback thresholds. For mission-critical data migrations, I'd use blue-green."

## Common combinations
- Feature Flags + Canary → controlled and safe
- Blue-Green + Canary → zero downtime + gradual exposure
- Rolling + Feature Flags → efficient + flexible

## Manager-level talking points
Automated rollback thresholds · SLIs/SLOs · deployment health checks ·
backward-compatible schema migrations · blast radius reduction · infra cost trade-offs.

## Related
[[concepts/twelve-factor-app]] (Factor 9: disposability enables rolling/canary) ·
[[components/kubernetes]] (default = rolling; supports canary with traffic splitting)
