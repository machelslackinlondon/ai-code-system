# Codex IDE Prompt System

Portable "Core Rules + Task Layer" instructions for AI coding agents.

## Load Order

Load only what is needed:

1. Always: `core/rules.md`, `core/engineering-principles.md`, `core/output-format.md`
2. Conditional:
   - TDD/behavior changes: `core/tdd-principles.md`
   - Refactor work: `core/refactoring-principles.md`
   - Architecture/design pattern decisions: `core/design-patterns.md`
3. One task layer:
   - `tasks/bug-fix.md`
   - `tasks/feature-build.md`
   - `tasks/project-bootstrap.md`
   - `tasks/refactor.md`
   - `tasks/performance-optimization.md`
4. Active task source:
   - placeholders from the current prompt, or
   - `runtime/active-task.md`
5. Continuity, if present: `runtime/session-notes.md`

If placeholders and `runtime/active-task.md` both exist, placeholders win for the current session.

## Precedence

1. Core safety rules
2. Current user request
3. Selected task layer
4. Active task details
5. Session notes

Task layers may specialize behavior but must not weaken core safety rules.

## Token Budget Rules

- Do not load README or VS Code guide files unless asked.
- Do not load every task file; load only the selected task layer.
- Do not load optional core modules unless relevant.
- Keep `runtime/session-notes.md` concise: decisions, changed files, validation, open items, risks, next step.
- Summarize context instead of copying long file contents into responses.
- Prefer links/paths over repeated pasted instructions.

## Active Task Fields

Standard fields: `TASK_NAME`, `TASK_TYPE`, `TASK_DESCRIPTION`, `IMPACT_LEVEL`, `AFFECTED_SYSTEMS`, `RELATED_FILES`, `CONSTRAINTS`, `NON_GOALS`, `INPUTS`, `EXPECTED_OUTPUT`, `VALIDATION_PLAN`, `ROLLBACK_PLAN`, `TDD_MODE`.

Project bootstrap fields: `IS_MONOREPO`, `SERVICES`, `APP_NAME`, `SQL_DATABASE`, `DEPLOYMENT_TARGET`, `DEFAULT_STACK`, `SCAFFOLD_REQUIREMENTS`.

## Agent Behavior

- Validate repository context before changing files.
- Classify impact before implementation.
- Use the smallest safe change.
- Avoid unrelated refactors.
- Follow repository conventions.
- Run targeted validation when feasible.
- Use TDD for bug fixes, behavior-changing features, and refactors with weak coverage when feasible.
- Report changes, validation, and residual risk.
- Update `runtime/session-notes.md` before finishing when continuity matters.

## Task Selection

- `bug-fix.md`: broken or unexpected behavior.
- `feature-build.md`: new behavior.
- `project-bootstrap.md`: new project, app, service, package, or monorepo.
- `refactor.md`: behavior-preserving structure change.
- `performance-optimization.md`: latency, throughput, memory, cost, or scalability.

For project bootstrap, default to Node.js, Fastify, TypeScript, Tailwind CSS, Fly.io, Docker, Jest with coverage thresholds, Cypress, Lighthouse, k6, OpenTelemetry, Prometheus/Grafana/Loki, GitHub Actions, MongoDB DAO, and PostgreSQL/MySQL DAO support. If `IS_MONOREPO=yes` and `SERVICES` is missing, ask before scaffolding.
