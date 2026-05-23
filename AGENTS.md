# Codex IDE Prompt System

This repository is a portable "Core Rules + Task Layer" prompt system for AI coding agents.

Use it by copying this folder into any repository, or by copying the relevant files into that repository's agent instruction location.

## Operating Model

Always combine:

1. Core rules: durable engineering behavior that applies to every task.
2. Task layer: task-specific behavior for the current type of work.
3. Active task details: the concrete task details for the current session.

The task layer may specialize behavior, but it must never weaken the core rules.

## Load Order

Read and apply files in this order:

1. `core/rules.md`
2. `core/engineering-principles.md`
3. `core/output-format.md`
4. Optional core modules, only when relevant:
   - `core/refactoring-principles.md`
   - `core/design-patterns.md`
5. Exactly one task archetype from `tasks/`:
   - `tasks/bug-fix.md`
   - `tasks/feature-build.md`
   - `tasks/project-bootstrap.md`
   - `tasks/refactor.md`
   - `tasks/performance-optimization.md`
6. Active task details from one of these sources:
   - `runtime/active-task.md`
   - AI extension UI placeholder values supplied in the current prompt
7. Continuity context, when present:
   - `runtime/session-notes.md`

If both `runtime/active-task.md` and AI extension UI placeholder values are provided, use the placeholder values as the active task for the current session.

## Precedence

When instructions conflict, follow this order:

1. Safety, production, data, security, and infrastructure constraints from `core/`.
2. The current user request, unless it would violate higher-priority safety constraints.
3. Task-specific behavior from `tasks/`.
4. Concrete active task details from `runtime/active-task.md` or AI extension UI placeholders.

If active task details are incomplete, make safe assumptions for low-risk work. Ask for clarification before medium/high-impact changes when missing information affects safety, correctness, data, infrastructure, authentication, cost, or production behavior.

## Active Task Sources

Use `runtime/active-task.md` when the task should be stored in the repository.

Use AI extension UI placeholders when the task should be supplied from a saved prompt, slash command, prompt template, or custom instruction. Placeholder-driven tasks should provide the same fields as `tasks/task-schema.md`.

Use `runtime/session-notes.md` to maintain continuity between prompts. When this file exists, read it before continuing work and update it before finishing whenever decisions, changed files, validation, risks, open questions, or next steps change.

Common placeholder fields:

- `TASK_NAME`
- `TASK_TYPE`
- `TASK_DESCRIPTION`
- `IMPACT_LEVEL`
- `AFFECTED_SYSTEMS`
- `RELATED_FILES`
- `CONSTRAINTS`
- `NON_GOALS`
- `INPUTS`
- `EXPECTED_OUTPUT`
- `VALIDATION_PLAN`
- `ROLLBACK_PLAN`

Project bootstrap placeholder fields:

- `IS_MONOREPO`
- `SERVICES`
- `APP_NAME`
- `SQL_DATABASE`
- `DEPLOYMENT_TARGET`
- `DEFAULT_STACK`
- `SCAFFOLD_REQUIREMENTS`

## Required Agent Behavior

- Validate relevant repository context before changing files.
- Classify the change impact before implementation.
- Use the smallest safe change that satisfies the task.
- Keep unrelated refactors out of feature and bug-fix work.
- Prefer repository-local conventions over new abstractions.
- Run targeted validation when feasible.
- Report what changed, what was validated, and any residual risk.
- Maintain `runtime/session-notes.md` as a concise checkpoint when it exists and the task spans multiple prompts.

## Task Selection Guide

- Use `bug-fix.md` when behavior is broken or failing.
- Use `feature-build.md` when adding new behavior.
- Use `project-bootstrap.md` when scaffolding a new project, service, app, package, or monorepo.
- Use `refactor.md` when preserving behavior while improving structure.
- Use `performance-optimization.md` when improving latency, throughput, memory use, cost, or scalability.

For `project-bootstrap.md`, default to Node.js, Fastify, TypeScript, Tailwind CSS, Fly.io, Docker, Jest, Cypress, Lighthouse, k6, OpenTelemetry, Prometheus/Grafana/Loki observability dashboards, GitHub Actions, MongoDB DAO support, and PostgreSQL/MySQL DAO support unless the active task says otherwise. If `IS_MONOREPO` is yes and `SERVICES` is missing, ask for the service list before scaffolding.

If more than one task type applies, choose the dominant risk:

- Correctness failure before feature work.
- Project scaffold decisions before implementation details.
- Production performance before cosmetic structure.
- Behavior-preserving refactor only when structure blocks safe implementation.
