# Core Rules + Task Layer + Skills Prompt System

This folder is a portable prompt system for Codex-style IDE agents.

It separates stable engineering rules from task-specific instructions and reusable skills:

- `core/` contains always-on behavior.
- `tasks/` contains reusable task archetypes.
- `.agents/skills/` contains project-local agent skills that mirror the task archetypes.
- `runtime/active-task.md` contains the current task instance when task details should live in the repo.
- `runtime/session-notes.md` preserves continuity between prompts.
- `prompts/` contains reusable prompt templates for continuing or checkpointing work.
- `docs/examples.md` contains longer prompt examples.
- `skills.sh.json` groups the project skills for the Skills.sh repository page.
- AI extension UI placeholders can provide the current task instance when task details should live in the prompt/session.
- `AGENTS.md` explains how an agent should assemble the layers.

For a fuller guide to using this folder across VS Code projects, see `USING-WITH-VSCODE.md`.

## Use In Any Repository

### Repository Root Mode

Copy these files into the target repository root:

- `AGENTS.md`
- `core/`
- `tasks/`
- `.agents/skills/`
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

### Skills Alternative Mode

Use this when your agent supports agent skills or when you want the Skills.sh-style workflow.

Skills are reusable capabilities defined by a `SKILL.md` file with YAML frontmatter. This repo keeps project-local skills in `.agents/skills/`, which is the Codex project path used by the `skills` CLI. The existing `tasks/` files remain available as the legacy/manual task-layer path.

Use one of these prompts:

```text
Use AGENTS.md.
Use the matching project skill from .agents/skills instead of a task layer.
```

```text
Use $feature-build to implement this feature using the repository's existing patterns.
```

For a known workflow, reference the skill directly:

```text
Use AGENTS.md.
Use .agents/skills/code-review/SKILL.md to review the current branch.
```

For local CLI use:

```bash
npx skills add . --agent codex --skill feature-build
npx skills use . --skill code-review
```

For GitHub-based installation after this repo is published:

```bash
npx skills add machelslackinlondon/ai-code-system --agent codex --skill create-jira-ticket
```

`skills.sh.json` is present only to group skills on the Skills.sh repository page. It does not change how the `skills` CLI installs skills or how any `SKILL.md` file behaves.

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

Available task archetypes include bug fixes, business requirements planning, code reviews, feature builds, project bootstrapping, refactors, performance optimization, and risk discovery.

Use `tasks/create-jira-ticket.md` when you want an agent to draft or create Jira issues through a configured Jira MCP server. See `docs/jira-ticket-automation.md` for setup, required fields, create modes, and automation examples.

The project-bootstrap task supports new single-project or monorepo scaffolds. See `tasks/project-bootstrap.md` for the default stack and scaffold expectations.

For project-bootstrap placeholder workflows, set `REQUIRES_PRE_SCAFFOLD_APPROVAL=yes` when you want the agent to propose project shape, services/packages, tool choices, trade-offs, and included/skipped capabilities before creating files.

TDD is automated through `core/tdd-principles.md`. Agents load it only when the task changes behavior, fixes a bug, or needs characterization tests before refactoring.

Assessment alignment is available through `core/assessment-alignment.md` for review/audit tasks. It is not loaded by default.

Use `tasks/risk-discovery.md` when you want a non-functional risk register with mitigation strategies, including design-for-failure, data consistency, and availability analysis.

Use `tasks/code-review.md` when you want to review branch commits or diffs for correctness, passing tests, security, performance, scalability, maintainability, and introduced risk before applying fixes.

Use `tasks/business-requirements-planning.md` when you want to turn business requirements into an implementation-ready plan, acceptance criteria, risks, and a recommended next task before coding.

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
one relevant .agents/skills/*/SKILL.md file when using skills mode
one relevant tasks/*.md file
runtime/active-task.md or prompt placeholders
runtime/session-notes.md only when continuing work
```

Do not load `README.md`, `USING-WITH-VSCODE.md`, `docs/examples.md`, every skill, every task file, or optional core files unless the task requires them.

Main low-cost mode prompt:

```text
Use AGENTS.md in low-token mode.
Load only required core files, one matching skill or task layer, and the active task source.
Do not load README.md, USING-WITH-VSCODE.md, examples, every task file, or optional modules unless needed.
```

Example for a known task:

```text
Use AGENTS.md in low-token mode.
Treat this as a risk-discovery task.
Load only required core files, .agents/skills/risk-discovery/SKILL.md or tasks/risk-discovery.md, and the active task source.
Do not load README.md, USING-WITH-VSCODE.md, examples, every task file, or optional modules unless needed.
```

## Maintenance Rule

When adding or changing prompt-system behavior, update the source-of-truth file first. Keep README and VS Code docs as references rather than duplicating long instructions.

## Examples

Longer session prompts, placeholder templates, task prompts, and project-bootstrap examples live in `docs/examples.md`.

Do not load docs/examples.md unless examples are requested.

## Adding New Task Types

Create a new file in `tasks/` using `tasks/task-template.md`.

Every task file should define:

- when to use it
- required behavior
- constraints
- validation expectations
- output expectations

Task files may add stricter behavior, but must not weaken `core/` rules.

## Adding New Skills

Create a new project-local skill under `.agents/skills/<skill-name>/SKILL.md`.

Every skill should define:

- YAML frontmatter with `name` and `description`
- when the skill should trigger
- required workflow behavior
- validation expectations
- output expectations

Keep skill bodies concise. Put only reusable capability guidance in skills; keep one-off task instance details in `runtime/active-task.md`, prompt placeholders, or the current user request.

When adding a skill that should appear on Skills.sh, add its slug to `skills.sh.json`.
