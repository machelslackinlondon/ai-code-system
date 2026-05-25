# Core Rules + Task Layer Prompt System

This folder is a portable prompt system for Codex-style IDE agents.

It separates stable engineering rules from task-specific instructions:

- `core/` contains always-on behavior.
- `tasks/` contains reusable task archetypes.
- `runtime/active-task.md` contains the current task instance when task details should live in the repo.
- `runtime/session-notes.md` preserves continuity between prompts.
- `prompts/` contains reusable prompt templates for continuing or checkpointing work.
- `docs/examples.md` contains longer prompt examples.
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

Available task archetypes include bug fixes, code reviews, feature builds, project bootstrapping, refactors, performance optimization, and risk discovery.

The project-bootstrap task supports new single-project or monorepo scaffolds. See `tasks/project-bootstrap.md` for the default stack and scaffold expectations.

For project-bootstrap placeholder workflows, set `REQUIRES_PRE_SCAFFOLD_APPROVAL=yes` when you want the agent to propose project shape, services/packages, tool choices, trade-offs, and included/skipped capabilities before creating files.

TDD is automated through `core/tdd-principles.md`. Agents load it only when the task changes behavior, fixes a bug, or needs characterization tests before refactoring.

Assessment alignment is available through `core/assessment-alignment.md` for review/audit tasks. It is not loaded by default.

Use `tasks/risk-discovery.md` when you want a non-functional risk register with mitigation strategies, including design-for-failure, data consistency, and availability analysis.

Use `tasks/code-review.md` when you want to review branch commits or diffs for correctness, passing tests, security, performance, scalability, maintainability, and introduced risk before applying fixes.

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

Do not load `README.md`, `USING-WITH-VSCODE.md`, `docs/examples.md`, every task file, or optional core files unless the task requires them.

Low-token prompt:

```text
Use AGENTS.md in low-token mode.
Load only the required core files, one task layer, and the active task source.
Use runtime/session-notes.md only if continuing previous work.
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
