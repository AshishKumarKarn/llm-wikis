---
title: Maintainability (Operability, Simplicity, Evolvability)
type: concept
chapters: [1]
tags: [maintainability, foundations, software-engineering]
status: solid
updated: 2026-05-16
---

# Maintainability (Operability, Simplicity, Evolvability)

## Definition

Maintainability is designing software so that the many people who work on it over
time — engineering and operations, maintaining behavior and adapting it — can do so
productively. The majority of software cost is ongoing maintenance, not initial
development. It decomposes into three design principles: **operability**,
**simplicity**, **evolvability**. *(DDIA Ch 1)*

## Why it matters

Most cost and pain in software is post-launch (bug fixing, ops, failure
investigation, platform changes, tech debt, new features). Good design minimizes that
pain and avoids us creating legacy software ourselves.

## How it works (the three principles)

- **Operability** — make life easy for ops. "Good operations can work around bad
  software, but good software can't run reliably with bad operations." Data systems
  aid this with: runtime visibility/monitoring, automation support, no dependence on
  individual machines (so nodes can be taken down), clear operational model ("if I do
  X, Y happens"), good defaults with override, self-healing + manual control,
  predictable behavior.
- **Simplicity** — fight **accidental complexity** (Moseley & Marks: complexity *not*
  inherent in the problem, only in the implementation), not functionality. Symptoms:
  state-space explosion, tight coupling, tangled dependencies, inconsistent naming,
  performance hacks, special-casing ("big ball of mud"). The best tool is a good
  **abstraction** (high-level languages hide machine code; SQL hides on-disk
  structures, concurrency, crash recovery). Finding good abstractions is hard,
  especially in distributed systems.
- **Evolvability** (a.k.a. extensibility/modifiability/plasticity) — make future
  change easy; requirements are in constant flux. Agile/TDD/refactoring operate at
  small scale; DDIA seeks evolvability at the *data-system* level (e.g. "refactoring"
  Twitter's timeline from approach 1 → 2). Tightly linked to simplicity and good
  abstractions.

## Trade-offs

- Error-minimizing interfaces vs. flexibility — too restrictive ⇒ people work around
  them, negating the benefit.
- Simplicity ≠ reduced functionality — only reduced *accidental* complexity.

## Related concepts

- [[reliability]], [[scalability]] — the other two pillars
- [[ch04-encoding-and-evolution]] — evolvability made concrete (schema evolution,
  rolling upgrades)

## Sources

DDIA Ch 1 ("Maintainability"). Refs: Moseley & Marks "Out of the Tar Pit"; Hickey
"Simple Made Easy"; Brooks "No Silver Bullet"; Foote & Yoder "Big Ball of Mud".
