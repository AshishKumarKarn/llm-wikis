---
title: Distributed Tracing
type: concept
tags: [observability, tracing, opentelemetry, jaeger, zipkin, concepts]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Distributed Tracing

Tracks the lifecycle of a single request as it propagates through multiple microservices.
Where logs end at a network boundary, tracing stitches together the full call chain
using a unique trace ID (see [[sources/scenario-questions-study-guide]]).

## Core concepts

| Concept | Definition |
|---|---|
| **Trace** | Complete journey of a request across all services |
| **Span** | Single unit of work (one service call, DB query, etc.) |
| **Trace ID** | Unique ID passed across all service boundaries via headers |
| **Span ID** | Unique ID for each operation within a trace |
| **Parent Span** | Caller's span ID — enables hierarchy/nesting |

```
Trace: Order Request (trace_id: abc123)
  Span 1: API Gateway        [0ms – 5ms]
  Span 2: Order Service      [5ms – 50ms]
    Span 3: DB Call          [10ms – 45ms]
  Span 4: Payment Service    [50ms – 120ms]
```

## How it works
1. **Trace context propagation:** each request carries `trace_id` + `span_id` in headers
   (W3C `traceparent` standard). Middleware/libraries ensure these propagate automatically.
2. **Spans:** created per operation; nested to show hierarchy and timing.
3. **Collectors:** services emit trace data to an OpenTelemetry Collector.
4. **Backends:** collector exports to Jaeger, Zipkin, or Grafana Tempo for visualization.

## Tool comparison
| Tool | Best for |
|---|---|
| **OpenTelemetry** | Standard instrumentation library; vendor-neutral; collect + export |
| **Jaeger** | Enterprise-scale; distributed; complex microservice topologies |
| **Zipkin** | Simpler; good for smaller teams getting started |
| **Grafana Tempo** | Integrates with Grafana dashboards + Loki logs |

**Recommendation:** OpenTelemetry + Jaeger for enterprise; Zipkin for small teams.

## Best practices
- Instrument all services (consistent context propagation)
- Correlate traces with logs and metrics for full observability
- **Sample wisely:** adaptive sampling to balance cost vs. visibility (don't trace 100%)
- Never include sensitive data (tokens, PII) in trace payloads
- Visualize service dependency maps to detect bottlenecks

## Challenges
- Overhead: adds latency and storage costs if not sampled properly
- Consistency: requires instrumentation across all heterogeneous services
- Data volume: high-traffic systems generate massive trace data; retention policies matter

## Related
[[scenarios/production-incident-response]] · [[components/elk-stack]] ·
[[scenarios/api-performance-optimization]]
