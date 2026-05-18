---
title: SOLID Principles
type: concept
tags: [solid, oop, design-principles, concepts]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# SOLID Principles

Five OOP design principles that make software easier to maintain, extend, and
test. Source pages (p72–77) are image-only; this page is written from general
knowledge (see [[sources/design-patterns-study-guide]]).

## The 5 Principles

### S — Single Responsibility Principle (SRP)
A class should have only one reason to change — one responsibility.
- **Bad:** `UserService` handles auth, email, and DB persistence.
- **Good:** `UserService`, `EmailService`, `UserRepository` separated.
- **Why it matters:** isolates change impact; easier testing.

### O — Open/Closed Principle (OCP)
Open for extension, closed for modification. Add new behavior by adding code,
not changing existing code.
- **Pattern:** Strategy, Decorator — add a new implementation rather than
  modifying the existing class.
- **Why it matters:** prevents regressions when adding features.

### L — Liskov Substitution Principle (LSP)
Subtypes must be substitutable for their base types without altering program
correctness. If `Square extends Rectangle`, callers using `Rectangle` must not
break when given a `Square`.
- **Why it matters:** inheritance hierarchies that violate LSP cause subtle bugs
  when polymorphism is used.

### I — Interface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they don't use.
Split fat interfaces into smaller, role-specific ones.
- **Bad:** `Animal` interface with `fly()`, `swim()`, `run()` forces `Dog` to
  implement `fly()`.
- **Good:** `Flyable`, `Swimmable`, `Runnable` separate.
- **Why it matters:** reduces coupling; easier mocking in tests.

### D — Dependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules. Both should depend
on abstractions. Abstractions should not depend on details.
- **Bad:** `OrderService` directly instantiates `MySQLRepository`.
- **Good:** `OrderService` depends on `OrderRepository` interface; MySQL
  implementation is injected (dependency injection).
- **Why it matters:** enables testability (mock the interface); swappable implementations.

## How SOLID relates to design patterns
| Principle | Supported by |
|---|---|
| OCP | Strategy, Decorator, Factory |
| DIP | Observer, Dependency Injection containers |
| ISP | Adapter (adapts fat interfaces) |

## Related
[[concepts/gof-patterns-overview]] · [[sources/design-patterns-study-guide]]
