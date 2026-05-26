# Business Requirements Planning Task Layer

Use this task layer when turning business requirements, product goals, client briefs, or vague feature ideas into an implementation plan before coding.

Default behavior is planning-only. Do not modify product code, scaffold files, or implementation files unless the user explicitly approves moving into an implementation task.

## Required Behavior

- Translate business goals into clear user/system outcomes.
- Identify assumptions, unknowns, constraints, non-goals, and decision points.
- Break the work into small feature slices or delivery phases.
- Define acceptance criteria that can be validated.
- Identify affected systems, data flows, dependencies, integrations, and operational concerns.
- Include risks and mitigation strategies, including security, data consistency, availability, performance, observability, rollout, and rollback where relevant.
- Recommend the next task type after planning, such as `feature-build`, `project-bootstrap`, `risk-discovery`, `performance-optimization`, or `code-review`.
- Ask for approval before implementation.

## Planning Checklist

- What business outcome should this deliver?
- Who are the users, actors, or systems involved?
- What must be true for the requirement to be considered complete?
- What is explicitly out of scope?
- What existing repository patterns or constraints affect the plan?
- What data, permissions, integrations, or operational paths are involved?
- What risks need mitigation before or during implementation?
- What is the smallest useful increment?

## Constraints

- Keep the output implementation-ready, but do not write implementation code by default.
- Do not invent business rules when requirements are unclear; mark them as assumptions or questions.
- Do not over-design the solution before validating the required outcome.
- Prefer phased delivery over a large all-at-once implementation.
- Use repository evidence when available; use `Unknown` when context is missing.

## Validation Expectations

- Define how each acceptance criterion should be validated.
- Identify tests, checks, manual verification, observability, or review steps needed for each phase.
- Call out validation gaps and follow-up discovery tasks.

## Output Expectations

- Business requirement summary
- Assumptions and unknowns
- User/system outcomes
- Proposed feature slices or phases
- Acceptance criteria
- Affected systems and dependencies
- Technical approach options and trade-offs
- Risks and mitigation strategies
- Validation strategy
- Recommended next task type
- Approval checkpoint before implementation
