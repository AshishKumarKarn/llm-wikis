---
title: Feign Client
type: pattern
tags: [feign, spring-cloud, http-client, microservices, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Feign Client

A declarative HTTP client in Spring Cloud that lets one microservice call another
service's REST API as if it were a normal Java method — zero manual HTTP handling
(see [[sources/design-patterns-study-guide]]).

## Core idea
You define a Java interface annotated with `@FeignClient` and endpoint mappings.
Feign auto-generates the HTTP client implementation behind the scenes.

```java
@FeignClient(name = "crew-service")
public interface CrewServiceClient {
    @GetMapping("/crew/{id}/availability")
    CrewStatus getAvailability(@PathVariable String id);
}

// Caller just does:
crewServiceClient.getAvailability("pilot-123");
```

## What Feign handles automatically
- Builds HTTP request (URL, method, headers)
- Serializes / deserializes JSON
- Integrates with Eureka for service discovery (resolves service name → instance)
- Integrates with Ribbon for client-side load balancing
- Supports fallback with Resilience4j
- Adds headers (JWT, tracing IDs, etc.)

## Why use it (vs RestTemplate / WebClient)
| Approach | Code | Load Balancing |
|---|---|---|
| RestTemplate | Boilerplate URL building, error handling | Manual |
| WebClient | Reactive; still boilerplate | Manual |
| **Feign** | Declarative interface | Auto via Ribbon/Eureka |

Feign is ideal for synchronous service-to-service calls. Use WebClient for reactive/async.

## When Feign is ideal
- SchedulingService → CrewService ("is this pilot available?")
- AuthService → UserProfileService
- ReportingService → FlightService
- Any service-to-service REST call in a Spring Cloud microservices stack

## Limitations
- Synchronous only (blocking) — for reactive use `spring-cloud-starter-openfeign` with WebFlux workarounds or prefer WebClient
- Tightly coupled to Spring ecosystem

## Related
[[patterns/service-discovery]] (Eureka) · [[patterns/circuit-breaker]] (Resilience4j fallback) ·
[[patterns/api-composition]] (Feign powers the fan-out calls)
