---
title: "Source: Scenario Based Questions Study Guide"
type: source
tags: [scenarios, debugging, microservices, jvm, interview]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Source: Scenario Based Questions Study Guide

**Origin:** `raw/Scenario Based Questions.pdf` · 99 pages · personal study notes
(OneNote export), Mar–May 2026 · interview prep (SA / EM / senior engineer focus).

## Summary
Three sections: (1) General — scenario Q&A (p1–61); (2) Database — slow writes (p62–65);
(3) Debugging — JVM tooling (p66–99).

## Section index → wiki pages
- API Performance Optimization (p1–3) → [[scenarios/api-performance-optimization]]
- Concurrency in Microservices (p4–10) → [[scenarios/concurrency-in-microservices]]
- Service-to-Service Interaction (p11–13) → [[patterns/api-composition]], [[patterns/feign-client]]
- Scheduled Jobs in Microservices (p14–17) → [[scenarios/scheduled-jobs-microservices]]
- Distributed Tracing (p18–20) → [[concepts/distributed-tracing]]
- Data Migration to Cloud (p21–24) → [[scenarios/data-migration-to-cloud]]
- REST API Error Handling (p25–28) → Spring Boot `@RestControllerAdvice` / `@ExceptionHandler`
- Security in Microservices (p29–30) → [[concepts/microservice-security]]
- Production Bug Handling (p31–32) → [[scenarios/production-incident-response]]
- 10 Common Scenario Q&As (p33–34) → [[scenarios/production-incident-response]]
- Fallback Strategy for App Migration (p55) → [[patterns/deployment-strategies]]
- Redis Race Conditions + Lua Script (p56–59) → [[components/redis]]
- Slow DB Writes (p62–65) → [[scenarios/slow-db-writes]]
- JVM Debugging — Heap/Thread Dumps, GC Logs (p66–99) → [[concepts/jvm-debugging]]

## Stub questions (headers only, no answer content)
p35: Design payment processing for millions of transactions/day
p36: Scale 10x traffic spike without downtime
p39: Multi-region availability during cloud outage
p40: Healthcare HIPAA/GDPR security
p41: Secure service-to-service communication
p42: API latency 200ms → 2s diagnosis
p43: Global latency optimization
p44: Monolith vs microservices for startup
p45: SQL vs NoSQL decision
p46: Engineering team Kubernetes adoption
p47: Negotiate unrealistic SLA
p48: Secure your microservice (header only)
p49: Prevent CSRF attack in microservice
p50: Production-grade Spring Boot microservice design
p51: Kafka vs RabbitMQ choice
p52: Web request-response lifecycle
p53: Lead production recovery
p54: Align backend architecture with business goals

## Key takeaways
1. Production debugging = Detect (metrics/traces) → Isolate (CB, rate-limit) → RCA (logs, dumps) → Fix (canary) → Prevent (chaos).
2. Redis is single-threaded per command but NOT per multi-step workflow — use INCR, Lua scripts, or distributed locks.
3. JVM debugging hierarchy: GC logs (patterns over time) → Heap dump (snapshot for OOM/leaks) → Thread dump (deadlocks/high CPU).
4. Data migration strategy: DMS for databases, DataSync for files, Snowball for offline bulk, all with CDC for continuous sync.
