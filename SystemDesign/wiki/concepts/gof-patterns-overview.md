---
title: GoF Design Patterns Overview
type: concept
tags: [gof, design-patterns, oop, concepts]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# GoF Design Patterns Overview

The 23 patterns from "Design Patterns: Elements of Reusable Object-Oriented Software"
(Gang of Four). Source pages (p78–101) are image-only; this page is written from
general knowledge (see [[sources/design-patterns-study-guide]]).

## Creational (5) — how objects are created
| Pattern | One-liner | Key use |
|---|---|---|
| **Singleton** | One instance globally | Config, logging, connection pool |
| **Factory Method** | Subclass decides which object to create | Decouple creation from use |
| **Abstract Factory** | Factory of factories | Platform-independent UI kits |
| **Builder** | Step-by-step object construction | Complex objects with many optional fields |
| **Prototype** | Clone existing objects | Expensive-to-create objects |

## Structural (7) — how objects are composed
| Pattern | One-liner | Key use |
|---|---|---|
| **Adapter** | Converts interface A to interface B | Integrate legacy/third-party APIs |
| **Bridge** | Separate abstraction from implementation | Cross-platform rendering |
| **Composite** | Tree structures of uniform objects | File system, UI component trees |
| **Decorator** | Add behavior without subclassing | HTTP middleware, Java I/O streams |
| **Facade** | Simplified interface over a subsystem | SDK hiding internal complexity |
| **Flyweight** | Share common state among many objects | Game objects, character rendering |
| **Proxy** | Placeholder with controlled access | Lazy load, auth, caching, remote |

## Behavioral (11) — how objects communicate
| Pattern | One-liner | Key use |
|---|---|---|
| **Chain of Responsibility** | Pass request along handler chain | Middleware pipelines, event filters |
| **Command** | Encapsulate request as object | Undo/redo, queued operations |
| **Interpreter** | Grammar for a language | SQL parsers, expression evaluators |
| **Iterator** | Sequential access without exposing internals | Collections traversal |
| **Mediator** | Centralize communication between objects | Chat rooms, air traffic control |
| **Memento** | Save and restore state | Undo history |
| **Observer** | Notify dependents on state change | Event systems, Kafka consumers, UI data binding |
| **State** | Change behavior based on internal state | Vending machine, order workflow |
| **Strategy** | Swap algorithms at runtime | Sorting, payment processors, auth |
| **Template Method** | Skeleton algorithm, subclass fills steps | Framework hooks |
| **Visitor** | Add operations to objects without modifying them | AST traversal, export formats |

## Most interview-relevant
- **Strategy** — aligns with [[concepts/solid-principles]] OCP; swap behavior at runtime.
- **Observer** — underpins event-driven systems; connects to [[patterns/event-sourcing]].
- **Decorator** — middleware chains; Java I/O; HTTP filter chains.
- **Proxy** — lazy loading, caching, auth gating.
- **Builder** — avoids telescoping constructors; used heavily in Java (e.g., `StringBuilder`, test builders).
- **Singleton** — often an antipattern in DI contexts; discuss with care.
- **Factory Method / Abstract Factory** — decouple creation; support [[concepts/solid-principles]] DIP.

## Related
[[concepts/solid-principles]] · [[sources/design-patterns-study-guide]]
