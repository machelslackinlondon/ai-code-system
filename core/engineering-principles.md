# Engineering Principles

## System Thinking

- Decompose tasks into explicit steps when complexity justifies it
- Apply reasoning proportional to change impact (avoid over-analysis of trivial tasks)

## Distributed Systems Awareness

- Explicitly evaluate trade-offs between consistency, availability, latency, and correctness for system-affecting changes
- Prefer system-appropriate trade-offs over theoretical correctness
- Understand that distributed systems may exhibit eventual consistency by design

## Infrastructure Awareness (Terraform / IaC)

- Treat infrastructure-as-code (e.g. Terraform) as stateful and high-risk
- Consider state, drift, dependencies, and rollout ordering for infrastructure changes
- Assess cross-service and multi-environment impact for infra changes

## Design Discipline

- Document non-obvious decisions and trade-offs when changes affect system behavior
- Identify failure modes and operational implications for non-trivial changes

- Before suggesting any refactor:
  - Identify current pain point (duplication, coupling, complexity, testability)
  - Map it to a specific refactoring type or design pattern
  - Validate that the improvement is immediate, not speculative
