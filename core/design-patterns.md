# Design Pattern Usage Guide

## Pattern Selection Rule

- Only propose a design pattern if it reduces complexity or coupling immediately
- Never introduce patterns for hypothetical future flexibility

---

## Common Pattern Triggers

### Factory Pattern

Use when:

- object creation logic is complex
- multiple implementations exist behind a single interface

Avoid when:

- only one implementation exists

---

### Strategy Pattern

Use when:

- multiple interchangeable behaviours exist
- frequent conditional branching selects behaviour

---

### Observer Pattern

Use when:

- multiple systems must react to state changes
- event-driven architecture is required

---

### Adapter Pattern

Use when:

- integrating incompatible interfaces or external services

---

### Repository Pattern

Use when:

- abstracting data access logic from business logic

---

## Anti-Pattern Rule

- Do not introduce patterns prematurely
- Prefer simple code over abstracted patterns unless complexity justifies it
