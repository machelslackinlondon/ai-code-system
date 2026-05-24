# Core Rules + Task Layer Prompt System

This folder is a portable prompt system for Codex-style IDE agents.

It separates stable engineering rules from task-specific instructions:

- `core/` contains always-on behavior.
- `tasks/` contains reusable task archetypes.
- `runtime/active-task.md` contains the current task instance when task details should live in the repo.
- `runtime/session-notes.md` preserves continuity between prompts.
- `prompts/` contains reusable prompt templates for continuing or checkpointing work.
- AI extension UI placeholders can provide the current task instance when task details should live in the prompt/session.
- `AGENTS.md` explains how an agent should assemble the layers.

For a fuller guide to using this folder across VS Code projects, see `USING-WITH-VSCODE.md`.

## Use In Any Repository

### Repository Root Mode

Copy these files into the target repository root:

- `AGENTS.md`
- `core/`
- `tasks/`
- `runtime/`

This lets Codex-style agents discover `AGENTS.md` automatically.

### Subfolder Mode

Copy this folder into the target repository under a stable name such as `prompt-system/`, then explicitly tell the agent:

```text
Follow prompt-system/AGENTS.md.
```

### AI Extension Placeholder Mode

Use this when your VS Code AI extension supports saved prompts, prompt templates, variables, slash commands, or custom instructions.

Keep `AGENTS.md`, `core/`, and `tasks/` in the project, then pass task values through prompt placeholders such as:

```text
Follow AGENTS.md.

TASK_TYPE:
{{TASK_TYPE}}

TASK_DESCRIPTION:
{{TASK_DESCRIPTION}}

IMPACT_LEVEL:
{{IMPACT_LEVEL}}

EXPECTED_OUTPUT:
{{EXPECTED_OUTPUT}}
```

For project bootstrapping, also include:

```text
IS_MONOREPO:
{{IS_MONOREPO}}

SERVICES:
{{SERVICES}}

SQL_DATABASE:
{{SQL_DATABASE}}

DEPLOYMENT_TARGET:
{{DEPLOYMENT_TARGET}}
```

When placeholders are supplied, they act as the active task for that session. `runtime/active-task.md` can remain a fallback template.

### Automated Context Mode

Use `runtime/session-notes.md` as a running checkpoint between prompts.

At the start of a follow-up prompt, use:

```text
Use AGENTS.md.
Continue from runtime/active-task.md and runtime/session-notes.md.
Update runtime/session-notes.md before finishing.
```

For saved prompts, use the templates in `prompts/`:

- `prompts/continue-task.md`
- `prompts/checkpoint-task.md`
- `prompts/placeholder-continue-task.md`

## Per-Task Use

1. Fill `runtime/active-task.md` with the current task, or provide task fields through AI extension UI placeholders.
2. Use `runtime/session-notes.md` when the task will span multiple prompts.
3. Pick one matching task archetype from `tasks/`.
4. Ask the coding agent to follow `AGENTS.md`.

Available task archetypes include bug fixes, feature builds, project bootstrapping, refactors, and performance optimization.

The project-bootstrap task supports new single-project or monorepo scaffolds. See `tasks/project-bootstrap.md` for the default stack and scaffold expectations.

For project-bootstrap placeholder workflows, set `REQUIRES_PRE_SCAFFOLD_APPROVAL=yes` when you want the agent to propose project shape, services/packages, tool choices, trade-offs, and included/skipped capabilities before creating files.

TDD is automated through `core/tdd-principles.md`. Agents load it only when the task changes behavior, fixes a bug, or needs characterization tests before refactoring.

Assessment alignment is available through `core/assessment-alignment.md` for review/audit tasks. It is not loaded by default.

## Low-Token Use

For day-to-day agent work, load only:

```text
AGENTS.md
core/rules.md
core/engineering-principles.md
core/output-format.md
core/problem-understanding.md
core/tdd-principles.md when behavior changes or coverage is weak
core/assessment-alignment.md only for review/audit tasks
one relevant tasks/*.md file
runtime/active-task.md or prompt placeholders
runtime/session-notes.md only when continuing work
```

Do not load `README.md`, `USING-WITH-VSCODE.md`, every task file, or optional core files unless the task requires them.

Low-token prompt:

```text
Use AGENTS.md in low-token mode.
Load only the required core files, one task layer, and the active task source.
Use runtime/session-notes.md only if continuing previous work.
```

## Maintenance Rule

When adding or changing prompt-system behavior, update the source-of-truth file first. Keep README and VS Code docs as references rather than duplicating long instructions.

## Recommended Session Prompt

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use runtime/active-task.md as the current task definition.
```

With placeholders:

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use the placeholder values in this prompt as the active task.
```

With automated continuity:

```text
Use this repository's AGENTS.md prompt system.
Use the active task details from runtime/active-task.md or the placeholders in this prompt.
Use runtime/session-notes.md as continuity context.
Before finishing, update runtime/session-notes.md with decisions, changed files, validation, open questions, open items, risks, and next steps.
```

Example placeholder session prompt:

```text
Use this repository's AGENTS.md prompt system.
Apply the core rules, select the relevant task layer, and use the placeholder values in this prompt as the active task.

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
```

Filled project-bootstrap example:

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

## Adding New Task Types

Create a new file in `tasks/` using `tasks/task-template.md`.

Every task file should define:

- when to use it
- required behavior
- constraints
- validation expectations
- output expectations

Task files may add stricter behavior, but must not weaken `core/` rules.
