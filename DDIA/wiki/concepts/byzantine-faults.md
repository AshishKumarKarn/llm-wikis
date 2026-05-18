---
title: Byzantine Faults
type: concept
chapters: [8]
tags: [distributed, byzantine, security, consensus]
status: developing
updated: 2026-05-16
---

# Byzantine Faults

## Definition

A node that **lies** — sends arbitrary faulty/corrupted responses, e.g. claims to
have received a message it didn't. Reaching consensus among nodes where some are
traitors is the **Byzantine Generals Problem** (generalizes the Two Generals
Problem). A system is **Byzantine fault-tolerant** if it operates correctly despite
malfunctioning/malicious nodes. *(DDIA Ch 8)*

## The book's assumption

DDIA assumes nodes are **unreliable but honest**: they may be slow, unresponsive, or
have stale state, but if they respond they follow the protocol. Byzantine tolerance
matters for: **aerospace** (radiation-corrupted memory), **multi-organization /
peer-to-peer** with no central authority (Bitcoin/blockchains — mutually-untrusting
parties agreeing without a central authority). In a trusted datacenter it's usually
**out of scope** (too complex/expensive; needs >2/3 correct nodes; same buggy
software on all nodes defeats it anyway; a compromised node likely means all
compromised — use authentication/encryption/firewalls instead). Web apps do treat
*clients* as adversarial (input validation, escaping) but make the **server the
authority**, not a Byzantine protocol.

## Weak forms of lying

Worth guarding against non-adversarial corruption: application-level **checksums**
(TCP/UDP checksums sometimes miss corruption), input sanitization/range checks,
**multi-server NTP** (outlier detection). Pragmatic reliability, not full Byzantine
tolerance.

## Related concepts

- [[fencing-tokens]] (only stops *inadvertent* error) · [[truth-by-majority]] ·
  [[system-models]] (Byzantine = arbitrary faults) ·
  [[ch09-consistency-and-consensus]]
- [[byzantine-generals-paper]]

## Sources

DDIA Ch 8 ("Byzantine Faults").
