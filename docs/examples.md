# Prompt Examples

Do not load docs/examples.md unless examples are requested.

This file keeps longer prompt examples out of the default onboarding docs so agents can stay in low-token mode during normal work.

## Basic Session Prompts

Use this when the task details live in `runtime/active-task.md`:

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use runtime/active-task.md as the current task definition.
```

Use this when task details are supplied through AI extension UI placeholders:

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use the placeholder values in this prompt as the active task.
```

Use this when continuing work across prompts:

```text
Use this repository's AGENTS.md prompt system.
Use the active task details from runtime/active-task.md or the placeholders in this prompt.
Use runtime/session-notes.md as continuity context.
Before finishing, update runtime/session-notes.md with decisions, changed files, validation, open questions, open items, risks, and next steps.
```

## Placeholder Task Prompt

Use this as a saved prompt template when your AI extension supports variables/placeholders.

```text
Follow AGENTS.md.

Use the Core Rules + Task Layer system.
Use the placeholder values in this prompt as the active task.
Apply the matching task layer from `tasks/`.

TASK_NAME:
{{TASK_NAME}}

TASK_TYPE:
{{TASK_TYPE}}

TASK_DESCRIPTION:
{{TASK_DESCRIPTION}}

IMPACT_LEVEL:
{{IMPACT_LEVEL}}

AFFECTED_SYSTEMS:
{{AFFECTED_SYSTEMS}}

RELATED_FILES:
{{RELATED_FILES}}

CONSTRAINTS:
{{CONSTRAINTS}}

NON_GOALS:
{{NON_GOALS}}

INPUTS:
{{INPUTS}}

EXPECTED_OUTPUT:
{{EXPECTED_OUTPUT}}

VALIDATION_PLAN:
{{VALIDATION_PLAN}}

ROLLBACK_PLAN:
{{ROLLBACK_PLAN}}

TDD_MODE:
{{TDD_MODE}}

Project bootstrap fields, if TASK_TYPE is project-bootstrap:

IS_MONOREPO:
{{IS_MONOREPO}}

SERVICES:
{{SERVICES}}

APP_NAME:
{{APP_NAME}}

SQL_DATABASE:
{{SQL_DATABASE}}

DEPLOYMENT_TARGET:
{{DEPLOYMENT_TARGET}}

DEFAULT_STACK:
{{DEFAULT_STACK}}

SCAFFOLD_REQUIREMENTS:
{{SCAFFOLD_REQUIREMENTS}}

NODE_VERSION_POLICY:
{{NODE_VERSION_POLICY}}

NODE_VERSION:
{{NODE_VERSION}}

CREATE_NVMRC:
{{CREATE_NVMRC}}

SWITCH_NODE_BEFORE_INSTALL:
{{SWITCH_NODE_BEFORE_INSTALL}}

REQUIRES_PRE_SCAFFOLD_APPROVAL:
{{REQUIRES_PRE_SCAFFOLD_APPROVAL}}

TOOL_DECISION_CRITERIA:
{{TOOL_DECISION_CRITERIA}}

PROJECT_SHAPE_DECISION:
{{PROJECT_SHAPE_DECISION}}

INCLUDE_CI_CD:
{{INCLUDE_CI_CD}}

INCLUDE_DEPLOYMENT:
{{INCLUDE_DEPLOYMENT}}

INCLUDE_DOCKER:
{{INCLUDE_DOCKER}}

INCLUDE_DATABASES:
{{INCLUDE_DATABASES}}

INCLUDE_OBSERVABILITY:
{{INCLUDE_OBSERVABILITY}}

If a placeholder is empty or unknown, treat it as `Unknown`.
Make safe assumptions only for low-risk work.
Ask for clarification before medium/high-impact work when missing information affects safety, correctness, production behavior, data, authentication, infrastructure, cost, or rollback.
```

For subfolder installs, change `Follow AGENTS.md.` to:

```text
Follow prompt-system/AGENTS.md.
Use the task details below instead of prompt-system/runtime/active-task.md for this session.
```

## Continuity Prompts

Use this prompt when continuing from repository task files:

```text
Use AGENTS.md.
Continue from runtime/active-task.md and runtime/session-notes.md.
Preserve the existing context.
Do not restart the task from scratch unless this request explicitly changes direction.
Before finishing, update runtime/session-notes.md with decisions, changed files, validation, open questions, open items, risks, and next steps.
```

Use this prompt when continuing from placeholder task values:

```text
Use AGENTS.md.
Use the placeholder values in this prompt as the active task.
Use runtime/session-notes.md as continuity context.
Before finishing, update runtime/session-notes.md.
```

## Active Task Example

```md
# Active Task

TASK_NAME:
Fix checkout total rounding issue

TASK_TYPE:
bug-fix

TASK_DESCRIPTION:
Checkout totals sometimes show a one-cent mismatch between the item subtotal and final charged amount.

IMPACT_LEVEL:
medium

AFFECTED_SYSTEMS:
Checkout, payments, order summary UI

RELATED_FILES:
Unknown

CONSTRAINTS:
Do not change payment provider integration behavior.

NON_GOALS:
Do not redesign checkout pricing.

INPUTS:
User report and failing checkout example.

EXPECTED_OUTPUT:
Root cause, minimal fix, regression test, validation summary.

VALIDATION_PLAN:
Run targeted checkout pricing tests.

ROLLBACK_PLAN:
Revert the checkout calculation change.
```

## Task Prompt Examples

Bug fix:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a bug-fix task.
```

Feature build:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a feature-build task.
```

Code review:

```text
Use AGENTS.md in low-token mode.
Treat this as a code-review task.
Review all branch changes from the merge-base/first divergent commit through REVIEW_HEAD_REF in the current repository.
Check correctness, scalability, maintainability, security, performance, data consistency, availability, and unit test status.
Classify introduced risk as low, medium, or high.
Do not apply fixes. Explain proposed changes and ask for approval before writing.
```

Project bootstrap:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a project-bootstrap task.
Use default stack and scaffold expectations from tasks/project-bootstrap.md unless the task says otherwise.
If this is a monorepo and services are not listed, ask for the service list before scaffolding.
```

Refactor:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a refactor task and preserve behavior.
```

Performance optimization:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a performance-optimization task and validate with measurements where feasible.
```

Risk discovery:

```text
Use AGENTS.md in low-token mode.
Treat this as a risk-discovery task.
Assess only the current repository/workspace unless I provide external architecture context.
Inspect non-functional limitations only; do not implement fixes.
Do not infer hidden systems; use Unknown for missing context.
Output a risk register with repo-backed evidence, severity, mitigation strategies, recommended actions, and follow-up tasks.
Include design-for-failure, data consistency, and availability analysis.
```

Contained risk discovery:

```text
Use AGENTS.md in low-token mode.
Treat this as a risk-discovery task.

Assess only the current repository/workspace.
Do not implement fixes.
Do not infer external architecture, production topology, or hidden services.
Use `Unknown` for missing context.

For each finding, cite evidence from repository files, configuration, observed behavior, or explicit context I provided.

Focus on:
- performance and scalability
- reliability and designing for failure
- data consistency and availability
- security and secrets
- observability
- CI/CD and deployment safety
- local development and test coverage

Output:
- executive summary
- risk register
- assumptions and unknowns
- mitigation strategies
- prioritized follow-up tasks
```

## Project Bootstrap Placeholder Example

Use this when creating a new project or monorepo:

```text
Use AGENTS.md.
Treat this as a project-bootstrap task.

TASK_NAME:
{{TASK_NAME}}

TASK_DESCRIPTION:
{{TASK_DESCRIPTION}}

IS_MONOREPO:
{{IS_MONOREPO}}

SERVICES:
{{SERVICES}}

APP_NAME:
{{APP_NAME}}

SQL_DATABASE:
{{SQL_DATABASE}}

DEPLOYMENT_TARGET:
Fly.io

DEFAULT_STACK:
Use `tasks/project-bootstrap.md` defaults.

SCAFFOLD_REQUIREMENTS:
Follow `tasks/project-bootstrap.md`; override only the fields in this prompt.

NODE_VERSION_POLICY:
{{NODE_VERSION_POLICY}}

NODE_VERSION:
{{NODE_VERSION}}

CREATE_NVMRC:
{{CREATE_NVMRC}}

SWITCH_NODE_BEFORE_INSTALL:
{{SWITCH_NODE_BEFORE_INSTALL}}

REQUIRES_PRE_SCAFFOLD_APPROVAL:
{{REQUIRES_PRE_SCAFFOLD_APPROVAL}}

TOOL_DECISION_CRITERIA:
{{TOOL_DECISION_CRITERIA}}

PROJECT_SHAPE_DECISION:
{{PROJECT_SHAPE_DECISION}}

INCLUDE_CI_CD:
{{INCLUDE_CI_CD}}

INCLUDE_DEPLOYMENT:
{{INCLUDE_DEPLOYMENT}}

INCLUDE_DOCKER:
{{INCLUDE_DOCKER}}

INCLUDE_DATABASES:
{{INCLUDE_DATABASES}}

INCLUDE_OBSERVABILITY:
{{INCLUDE_OBSERVABILITY}}

EXPECTED_OUTPUT:
Pre-scaffold proposal first when approval is required, then working scaffold after approval.

VALIDATION_PLAN:
Validate generated scripts, config files, tests, performance tests, observability setup, Docker setup, and README instructions. Run checks when dependencies are available.
```

If `IS_MONOREPO` is `yes`, provide `SERVICES` as a comma-separated or line-separated list:

```text
api
web
worker
shared
```

## Filled Project Bootstrap Example

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use the placeholder values in this prompt as the active task.

TASK_NAME:
Bootstrap customer portal platform

TASK_TYPE:
project-bootstrap

TASK_DESCRIPTION:
Create a new monorepo scaffold for a customer portal with API, web app, worker, and shared package.

IMPACT_LEVEL:
medium

AFFECTED_SYSTEMS:
New repository scaffold, local development, CI/CD, Docker, Fly.io deployment

RELATED_FILES:
Unknown

CONSTRAINTS:
Use environment variables for all secrets. Do not hard-code credentials. Keep load tests safe for local development by default.

NON_GOALS:
Do not implement full product features or production database migrations.

INPUTS:
Initial scaffold only.

EXPECTED_OUTPUT:
Working monorepo scaffold following `tasks/project-bootstrap.md`, with Docker setup, tests, CI/CD, Fly.io deployment docs, and detailed README.

VALIDATION_PLAN:
Validate scripts, config files, tests, Docker setup, CI workflow, deployment docs, and README instructions. Run checks where dependencies are available.

ROLLBACK_PLAN:
Revert the scaffold commit or remove generated project files.

IS_MONOREPO:
yes

SERVICES:
api
web
worker
shared

APP_NAME:
customer-portal

SQL_DATABASE:
PostgreSQL

DEPLOYMENT_TARGET:
Fly.io

DEFAULT_STACK:
Use `tasks/project-bootstrap.md` defaults. SQL DAO: PostgreSQL.

SCAFFOLD_REQUIREMENTS:
Follow `tasks/project-bootstrap.md`; create api, web, worker, and shared packages with PostgreSQL SQL DAO support.

NODE_VERSION_POLICY:
latest-lts

NODE_VERSION:
Unknown

CREATE_NVMRC:
yes

SWITCH_NODE_BEFORE_INSTALL:
yes

REQUIRES_PRE_SCAFFOLD_APPROVAL:
yes

TOOL_DECISION_CRITERIA:
Prefer boring, production-ready tools. Minimize framework complexity. Explain alternatives when they materially change cost, speed, or maintainability.

PROJECT_SHAPE_DECISION:
Recommend monorepo vs single app from the task description and explain reasoning before scaffolding.

INCLUDE_CI_CD:
yes

INCLUDE_DEPLOYMENT:
yes

INCLUDE_DOCKER:
yes

INCLUDE_DATABASES:
yes

INCLUDE_OBSERVABILITY:
yes
```
