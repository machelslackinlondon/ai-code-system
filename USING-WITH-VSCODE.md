# Using This Prompt System With VS Code Projects

This folder is a reusable prompt system for AI coding agents working inside VS Code.

It gives the agent a stable engineering policy, then layers the current task on top of that policy. The goal is to make the agent behave consistently across different repositories without rewriting instructions every time.

## What This Folder Does

The folder defines a "Core Rules + Task Layer" system.

- `core/` contains rules that should apply to every coding task.
- `tasks/` contains reusable instructions for common task types.
- `runtime/active-task.md` describes the specific task being worked on right now when task details should live in the repository.
- AI extension UI placeholders can describe the task when task details should live in the prompt/session.
- `AGENTS.md` tells the agent how to combine those files.

In practice, this means the agent first learns the permanent engineering standards, then applies the task-specific workflow, then uses either the active task file or placeholder values as the current work order.

## Why This Is Useful

Most coding tasks need two kinds of guidance:

1. Stable rules that should not change between projects.
2. Task-specific instructions that change depending on whether the agent is fixing a bug, building a feature, refactoring, or optimizing performance.

This folder keeps those concerns separate.

That makes it easier to:

- reuse the same agent behavior across multiple codebases
- reduce prompt repetition
- keep production-safety rules consistent
- give clearer instructions for each task type
- avoid accidental broad refactors during small tasks
- make validation and risk reporting part of the normal workflow

## Folder Structure

```text
AGENTS.md
README.md
USING-WITH-VSCODE.md

core/
  rules.md
  engineering-principles.md
  output-format.md
  tdd-principles.md
  refactoring-principles.md
  design-patterns.md

tasks/
  bug-fix.md
  feature-build.md
  project-bootstrap.md
  refactor.md
  performance-optimization.md
  task-schema.md
  task-template.md

runtime/
  active-task.md
  session-notes.md

prompts/
  continue-task.md
  checkpoint-task.md
  placeholder-continue-task.md
```

## How The Layers Work

### 1. Core Rules

The `core/` files define always-on behavior.

They tell the agent to:

- validate repository context before making changes
- treat production, data, authentication, and infrastructure changes carefully
- prefer small, reversible changes
- avoid speculative refactors
- consider reliability, performance, cost, maintainability, and rollback
- classify impact before implementation

These files should be stable and rarely changed.

`core/tdd-principles.md` is loaded only when the task changes behavior, fixes a bug, or needs characterization tests before refactoring.

### 2. Task Layer

The `tasks/` files define reusable workflows for common work types.

Use one task layer per task:

- `bug-fix.md` for diagnosing and correcting broken behavior
- `feature-build.md` for adding new behavior
- `project-bootstrap.md` for scaffolding new projects, apps, services, packages, or monorepos
- `refactor.md` for behavior-preserving structural improvement
- `performance-optimization.md` for latency, throughput, memory, cost, or scalability work

The task layer should specialize the workflow, not override the core rules.

### 3. Active Task

The active task describes the specific work for the current session.

It can come from either:

- `runtime/active-task.md`
- AI extension UI placeholder values in the current prompt

Use `runtime/active-task.md` when the task should be visible and versionable in the repository. Use placeholders when the task should stay in the AI extension session.

The active task can include:

- task name
- task type
- description
- impact level
- affected systems
- related files
- constraints
- non-goals
- expected output
- validation plan
- rollback plan

For project bootstrap tasks, it can also include:

- monorepo flag
- service list
- app name
- SQL database choice
- deployment target
- scaffold requirements

### 4. Session Notes

The `runtime/session-notes.md` file preserves continuity between prompts.

Use it when a task takes more than one prompt or when you want the next session to resume from a known checkpoint.

It should capture:

- current goal
- decisions made
- files changed
- validation run
- open questions
- open items
- risks or constraints
- next steps

## Using It In Another VS Code Codebase

There are three recommended ways to use this folder.

## Option A: Copy To The Repository Root

Use this when you want the prompt system to become part of the target repository.

Copy these files and folders into the root of the target codebase:

```text
AGENTS.md
core/
tasks/
runtime/
```

Then open that repository in VS Code.

When starting a task, fill `runtime/active-task.md`, then ask the agent:

```text
Use AGENTS.md. Apply the core rules, select the relevant task layer, and use runtime/active-task.md as the current task.
```

This mode is best when:

- the whole team should share the same agent instructions
- the prompt system should be versioned with the codebase
- the project has recurring AI-assisted engineering tasks

## Option B: Copy As A Subfolder

Use this when you want to keep the prompt system separate from the target repository's root files.

Copy this folder into the target codebase under a stable name:

```text
prompt-system/
```

Then open the target repository in VS Code and tell the agent:

```text
Follow prompt-system/AGENTS.md.
Use prompt-system/runtime/active-task.md as the current task definition.
```

This mode is best when:

- you are experimenting before adopting the system permanently
- the target repository already has its own root agent instructions
- you want to reuse the same prompt pack across several repositories

## Option C: Use AI Extension UI Placeholders

Use this when your VS Code AI extension lets you save reusable prompts, prompt templates, variables, slash commands, or custom instructions.

In this mode, you keep the prompt system files in the repository, but define each task through the extension UI instead of editing `runtime/active-task.md` for every task.

Placeholder values override `runtime/active-task.md` for the current session. The file can remain a fallback template.

Save a reusable prompt like this:

```text
Follow AGENTS.md.

Use the Core Rules + Task Layer system.

Task details:

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

Project bootstrap details, if applicable:

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

Apply the matching task layer from `tasks/`.
If a placeholder is empty or unknown, treat it as `Unknown`.
Make safe assumptions only for low-risk work.
Ask for clarification before medium/high-impact work when missing information affects safety, correctness, production behavior, data, authentication, infrastructure, cost, or rollback.
```

If the prompt system is copied as a subfolder, use the subfolder path:

```text
Follow prompt-system/AGENTS.md.

Use the task details below instead of prompt-system/runtime/active-task.md for this session.

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

Project bootstrap details, if applicable:

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
```

This mode is best when:

- your AI extension supports saved prompts or variables
- you want to start tasks quickly without editing files
- you want task details to stay in the chat/session rather than the repository
- different developers need their own task prompts while sharing the same core rules

When using this option, the placeholder values provided in the AI extension UI are the active task for that session.

## Option D: Automate Continuity With Session Notes

Use this when a task may span multiple prompts.

The automation pattern is:

1. Start from `AGENTS.md`.
2. Read the active task from `runtime/active-task.md` or AI extension placeholders.
3. Read `runtime/session-notes.md`.
4. Continue from the open items.
5. Before finishing, update `runtime/session-notes.md`.

Use this prompt:

```text
Use AGENTS.md.
Continue from runtime/active-task.md and runtime/session-notes.md.
Preserve the existing context.
Do not restart the task from scratch unless this request explicitly changes direction.
Before finishing, update runtime/session-notes.md with decisions, changed files, validation, open questions, open items, risks, and next steps.
```

If using placeholders:

```text
Use AGENTS.md.
Use the placeholder values in this prompt as the active task.
Use runtime/session-notes.md as continuity context.
Before finishing, update runtime/session-notes.md.
```

Saved prompt templates are available in `prompts/`.

## Per-Task Workflow

For each task:

1. Open `runtime/active-task.md`, or open your saved AI extension prompt.
2. Fill in the task details or prompt placeholders.
3. Use `runtime/session-notes.md` when the task will continue across prompts.
4. Choose the closest task type.
5. Start the agent with the recommended prompt.
6. Let the agent inspect the repository before changing files.
7. Review the final summary, validation, and residual risks.

If using AI extension UI placeholders, fill the prompt variables in the extension instead of editing `runtime/active-task.md`.

Example `runtime/active-task.md`:

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

## Recommended Prompts

For a bug fix:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a bug-fix task.
```

For a feature:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a feature-build task.
```

For a new project scaffold:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a project-bootstrap task.
Default to Node.js, Fastify, TypeScript, Tailwind, Fly.io, Docker, Jest, GitHub Actions, Cypress, Lighthouse, k6, OpenTelemetry, Prometheus/Grafana/Loki observability dashboards, MongoDB, and PostgreSQL/MySQL DAO support unless the task says otherwise.
If this is a monorepo and services are not listed, ask for the service list before scaffolding.
```

For a refactor:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a refactor task and preserve behavior.
```

For performance work:

```text
Use AGENTS.md and runtime/active-task.md. Treat this as a performance-optimization task and validate with measurements where feasible.
```

## How To Choose The Task Type

Choose the task type based on the dominant risk:

- If behavior is wrong, use `bug-fix`.
- If new behavior is being added, use `feature-build`.
- If a new project, app, service, package, or monorepo is being created, use `project-bootstrap`.
- If behavior must stay the same while structure improves, use `refactor`.
- If resource usage, latency, throughput, or cost is the main concern, use `performance-optimization`.

When a task overlaps categories, choose the one with the highest operational risk. For example, fixing a production performance regression should use `performance-optimization`, even if it also involves refactoring. Creating a new service inside an existing monorepo should use `project-bootstrap`, then apply feature-build behavior inside that scaffold.

## Project Bootstrap Prompt Example

Use this prompt when creating a new project or monorepo:

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
Node.js, Fastify, TypeScript, Tailwind, Docker, Jest, Cypress, Lighthouse, k6, OpenTelemetry, Prometheus/Grafana/Loki, GitHub Actions, Fly.io, MongoDB DAO, PostgreSQL/MySQL DAO

SCAFFOLD_REQUIREMENTS:
Default to Node.js, Fastify, TypeScript, Tailwind, Docker, Jest, Cypress, Lighthouse web performance tests, k6 load tests, OpenTelemetry instrumentation, Prometheus/Grafana/Loki observability dashboards, GitHub Actions, Fly.io deployment, MongoDB DAO, and PostgreSQL/MySQL DAO support.

EXPECTED_OUTPUT:
Working scaffold, Docker setup, tests, performance testing scripts, observability dashboard setup, CI/CD workflow, Fly.io deployment config, and detailed README documentation for running and deploying the application.

VALIDATION_PLAN:
Validate generated scripts, config files, tests, performance tests, observability setup, Docker setup, and README instructions. Run checks when dependencies are available.
```

If `IS_MONOREPO` is `yes`, provide `SERVICES` as a comma-separated or line-separated list, for example:

```text
api
web
worker
shared
```

If `IS_MONOREPO` is `yes` and `SERVICES` is empty, the agent should ask for the service list before creating files.

## What To Commit To Another Project

For most projects, commit:

```text
AGENTS.md
core/
tasks/
runtime/active-task.md
runtime/session-notes.md
prompts/
```

Do not copy editor-specific files unless the target project needs them.

The `.vscode/` folder in this repository is not required for the prompt system.

If your team uses AI extension UI placeholders, keep the saved prompt template in the extension or in your team's preferred documentation location. The prompt values themselves do not need to be committed unless the task should be auditable from the repository.

## Maintaining The Prompt System

Keep these rules in mind when editing the system:

- Put permanent safety and engineering rules in `core/`.
- Put reusable task workflows in `tasks/`.
- Put repository-visible task details in `runtime/active-task.md`.
- Put continuity checkpoints in `runtime/session-notes.md`.
- Put session-only task details in AI extension UI placeholders.
- Do not put project-specific implementation details in `core/`.
- Do not let task instructions weaken core safety constraints.
- Keep task files short enough for an agent to load and apply reliably.

## Quick Start

1. Copy `AGENTS.md`, `core/`, `tasks/`, and `runtime/` into a VS Code project.
2. Fill `runtime/active-task.md`, or use AI extension UI placeholders.
3. Use `runtime/session-notes.md` for tasks that span multiple prompts.
4. Ask the agent to follow `AGENTS.md`.
5. Work one task at a time.

## Low-Token Mode

For normal coding tasks, tell the agent:

```text
Use AGENTS.md in low-token mode.
Load only the required core files, one task layer, and the active task source.
Do not load README.md, USING-WITH-VSCODE.md, every task file, or optional core modules unless needed.
```

This keeps the active context small while preserving the same rules.
