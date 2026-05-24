# Task Schema

Use this schema for `runtime/active-task.md` and concrete task descriptions. Use `tasks/task-template.md` when creating a new reusable task archetype.

## Required Fields

TASK_NAME:

TASK_TYPE:

- bug-fix
- feature-build
- project-bootstrap
- refactor
- performance-optimization
- other

TASK_DESCRIPTION:

IMPACT_LEVEL:

- low
- medium
- high

AFFECTED_SYSTEMS:

RELATED_FILES:

CONSTRAINTS:

NON_GOALS:

INPUTS:

EXPECTED_OUTPUT:

VALIDATION_PLAN:

ROLLBACK_PLAN:

TDD_MODE:

- auto
- required
- skip

## Project Bootstrap Fields

Use these fields when `TASK_TYPE` is `project-bootstrap`.

IS_MONOREPO:

- yes
- no
- unknown

SERVICES:

APP_NAME:

SQL_DATABASE:

- PostgreSQL
- MySQL
- unknown

DEPLOYMENT_TARGET:

DEFAULT_STACK:

SCAFFOLD_REQUIREMENTS:

REQUIRES_PRE_SCAFFOLD_APPROVAL:

- yes
- no
- auto

TOOL_DECISION_CRITERIA:

PROJECT_SHAPE_DECISION:

INCLUDE_CI_CD:

- yes
- no
- auto

INCLUDE_DEPLOYMENT:

- yes
- no
- auto

INCLUDE_DOCKER:

- yes
- no
- auto

INCLUDE_DATABASES:

- yes
- no
- auto

INCLUDE_OBSERVABILITY:

- yes
- no
- auto

## Field Rules

- `TASK_DESCRIPTION` must describe the desired outcome, not only the implementation idea.
- `IMPACT_LEVEL` must follow `core/output-format.md`.
- `AFFECTED_SYSTEMS` should include services, modules, databases, APIs, queues, jobs, infrastructure, or user-facing flows.
- `RELATED_FILES` may be empty at task start, but the agent should update its working context by inspecting the repository.
- `CONSTRAINTS` should include compatibility, security, data, latency, rollout, style, or dependency limits.
- `NON_GOALS` prevents unrelated expansion.
- `VALIDATION_PLAN` should name tests, checks, manual verification, or the reason validation is not available.
- `ROLLBACK_PLAN` is required for medium/high-impact changes.
- `TDD_MODE` defaults to `auto`. Use `required` to force red-green-refactor when feasible, or `skip` only when tests are impractical or out of scope.
- For `project-bootstrap`, `IS_MONOREPO` must be known before scaffolding. If it is `yes`, `SERVICES` must list each service/package to create.
- For `project-bootstrap`, use `tasks/project-bootstrap.md` as the source of truth for default stack and scaffold expectations.
- For `project-bootstrap`, `REQUIRES_PRE_SCAFFOLD_APPROVAL` defaults to `auto`; treat `auto` as `yes` for monorepos and medium/high-impact scaffolds.
- For `project-bootstrap`, `INCLUDE_*` fields default to `auto`; use task description and constraints to decide, then explain included/skipped options in the pre-scaffold proposal.
