---
title: REST API Design
type: concept
tags: [rest, api, versioning, hateoas, openapi, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# REST API Design

REST (Representational State Transfer) principles for building scalable, maintainable
APIs. Covers the 6 constraints, Richardson Maturity Model, versioning strategies, and
practical design patterns (see [[sources/miscellaneous-study-guide]]).

## The 6 REST Constraints

| Constraint | Rule | Example |
|---|---|---|
| **Client-Server** | Client handles UI; server handles data/logic | Separate React app from Spring Boot API |
| **Stateless** | Each request contains all info; server holds no session state | `Authorization: Bearer <token>` per request |
| **Resource-Based** | Everything is a resource identified by URI; use nouns | `/users/123` not `/getUser/123` |
| **Uniform Interface** | Standard HTTP verbs (GET/POST/PUT/PATCH/DELETE) | `GET /orders`, `DELETE /orders/123` |
| **Cacheable** | Responses declare cacheability | `Cache-Control: max-age=3600` |
| **Layered System** | Clients don't know if they're talking to proxy, LB, or gateway | NGINX, Kong in front |
| **Code on Demand** *(optional)* | Server can send executable code | JavaScript to browser — rarely used |

## Richardson Maturity Model (RMM)

| Level | Name | Description | Example |
|---|---|---|---|
| 0 | Swamp of POX | One endpoint; HTTP as transport only | `POST /api { "action": "getUser" }` |
| 1 | Resources | Multiple resource-based URIs | `GET /users/123` |
| 2 | HTTP Verbs | Correct use of GET, POST, PUT, DELETE | `DELETE /users/123` |
| 3 | HATEOAS | Responses include links to next actions | `{ "links": [{ "rel": "orders", "href": "/users/123/orders" }] }` |

**Most real-world APIs are Level 2.** Level 3 (HATEOAS) is rarely implemented fully due to
complexity. Level 0 is an antipattern.

Architect insight: RMM is not about always reaching Level 3 — it's about choosing the
right level based on system complexity and client needs.

## REST API Versioning

When APIs evolve, versioning ensures backward compatibility.

| Strategy | How | Example | Verdict |
|---|---|---|---|
| **URI versioning** | Version in URL path | `/api/v1/users` | Most common; visible; URL changes |
| **Header versioning** | `API-Version: v1` header | `GET /users + header` | Clean URLs; harder to test manually |
| **Media type** | Accept header | `Accept: application/vnd.co.v1+json` | Follows HTTP standards; complex |
| **Query param** | `?version=1` | `/api/users?version=1` | Easy; not commonly used in production |

Best practices:
- Maintain backward compatibility; avoid breaking existing clients.
- Only version when necessary — adding new fields usually doesn't require a new version.
- Deprecate old versions gradually with advance notice.
- Document each version with Swagger/OpenAPI Specification.

## API Layering
```
Client
  ↓ API Gateway (routing, auth, rate-limiting)
  ↓ Controller / API Layer (request/response mapping)
  ↓ Service / Business Logic Layer
  ↓ Data Access Layer (repository, ORM)
  ↓ Database
```

## OpenAPI / Swagger
Standard for describing REST APIs. Key benefits:
- Standardized, auto-generated documentation.
- Client code generation from the spec.
- Contract-first development (frontend and backend agree on API contract before implementation).

## Related
[[components/api-gateway]] · [[patterns/api-composition]] ·
[[concepts/microservice-security]] (auth at gateway)
