---
title: "RPC vs. REST (and SOAP)"
type: comparison
chapters: [4]
tags: [services, rpc, rest, soap, dataflow]
status: solid
updated: 2026-05-16
---

# RPC vs. REST (and SOAP)

When HTTP is the transport for a service it's a **web service**. Two camps: REST and
SOAP. *(DDIA Ch 4)*

## REST vs. SOAP

- **REST** — not a protocol but a design philosophy on HTTP: simple formats, URLs as
  resources, HTTP for caching/auth/content negotiation. APIs are "RESTful";
  describable with OpenAPI/Swagger. Predominant for public APIs and microservices.
- **SOAP** — XML-based RPC protocol, HTTP-independent, sprawling WS-* standards, API
  described by WSDL (codegen, not human-readable). Interop pain across vendors;
  fading outside large enterprises.

## Why RPC's core idea is "fundamentally flawed"

RPC (since the 1970s) tries to make a remote call look like a local one (**location
transparency**). But a network request differs fundamentally:

- Unpredictable (loss, slow/unavailable remote) — must anticipate retries.
- Extra outcome: **timeout with no result** — you don't know if it executed (→
  [[ch08-the-trouble-with-distributed-systems]]).
- Retries without **idempotence** cause duplicate execution (→
  [[ch11-stream-processing]]).
- Latency is wildly variable; large objects must be encoded (no pointers);
  cross-language type mismatches (JS > 2^53).

## Modern RPC

Doesn't hide the network: gRPC (Protobuf), Thrift, Finagle (Thrift), Rest.li (JSON/
HTTP); futures/promises, streams, service discovery (→ Ch 6 "Request Routing").
Custom binary RPC beats JSON/REST on performance; REST wins on
debuggability/tooling/ubiquity → REST for public/cross-org, RPC for intra-org
same-datacenter.

## Compatibility

Assume **servers upgrade before clients** ⇒ need backward compat on requests,
forward compat on responses ([[backward-forward-compatibility]]). Cross-org clients
can't be forced to upgrade → maintain versions for a long time (URL/Accept-header
versioning).

## Related

- [[modes-of-dataflow]] · [[backward-forward-compatibility]] ·
  [[message-passing-dataflow]]
- [[ch08-the-trouble-with-distributed-systems]] · [[ch11-stream-processing]]

## Sources

DDIA Ch 4 ("Dataflow Through Services"). Refs: Fielding (REST thesis 2000); Waldo
et al. "A Note on Distributed Computing" (1994); Birrell & Nelson (RPC, 1984).
