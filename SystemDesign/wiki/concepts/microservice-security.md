---
title: Microservice Security
type: concept
tags: [security, oauth2, mtls, rbac, secrets, zero-trust, concepts]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Microservice Security

Layered security approach: authentication/authorization → Zero Trust → secure
communication → secure CI/CD → monitoring
(see [[sources/scenario-questions-study-guide]]).

## Security layers

| Layer | Practice | Tools |
|---|---|---|
| **Identity** | OAuth2/OpenID Connect, RBAC, centralized auth | Keycloak, Okta, Spring Security |
| **Communication** | mTLS between services, encrypted traffic | Istio, Linkerd, AWS App Mesh |
| **Gateway** | API Gateway + WAF; DDoS/injection protection | Kong, NGINX, AWS API Gateway |
| **Secrets** | Secure storage + rotation; never in code/env vars | HashiCorp Vault, AWS Secrets Manager |
| **CI/CD** | SAST/DAST scans, dependency checks | Snyk, OWASP Dependency-Check |
| **Monitoring** | Centralized logs, anomaly detection | ELK/Loki, Prometheus + Grafana |

## Core practices

### 1. Authentication & Authorization
- **Centralized Identity Provider** (Keycloak, Okta): OAuth2/OpenID Connect.
- **Edge-level auth:** enforce at API Gateway before requests reach services.
- **Service-to-service auth:** tokens or mTLS between every service pair.

### 2. Zero Trust
Never assume implicit trust between services. Every request is verified.
- Check for XSS, CSRF on all inputs.
- **mTLS** — both client and server authenticate; all traffic encrypted.
- **RBAC** — least privilege per service; no service has more access than needed.

### 3. Defense in Depth
- API Gateway + WAF: protect against injection, DDoS, malicious payloads.
- Network segmentation: isolate critical services in private subnets.
- Secrets management: Vault / AWS Secrets Manager; rotate automatically.

### 4. Secure Development Lifecycle
- Integrate SAST/DAST in CI/CD pipeline.
- `Snyk` or `OWASP Dependency-Check` for vulnerable dependencies.
- Scan Docker images for vulnerabilities before pushing to registry.

### 5. Monitoring & Incident Response
- Centralized logging with ELK or Loki.
- Prometheus + Grafana for real-time anomaly detection.
- IDS/IPS for suspicious traffic patterns.

## Challenges
- Over-centralized identity = single point of failure.
- mTLS adds certificate management overhead (solved by service mesh auto-rotation).
- Strict RBAC can slow development if roles aren't well-defined upfront.
- Security scans increase build times — accept the cost.

## Spring Boot implementation path
1. Spring Security + OAuth2 for centralized authentication.
2. Istio or AWS App Mesh for mTLS in service-to-service communication.
3. AWS Secrets Manager (not environment variables) for credentials.
4. Snyk or OWASP in CI/CD pipeline.
5. Grafana dashboards for anomaly detection.

## Related
[[components/service-mesh]] (mTLS, RBAC at mesh level) · [[components/api-gateway]] ·
[[components/elk-stack]] · [[concepts/distributed-tracing]]
