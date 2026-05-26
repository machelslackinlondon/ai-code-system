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
  problem-understanding.md
  tdd-principles.md
  assessment-alignment.md
  refactoring-principles.md
  design-patterns.md

tasks/
  bug-fix.md
  business-requirements-planning.md
  code-review.md
  feature-build.md
  project-bootstrap.md
  refactor.md
  performance-optimization.md
  risk-discovery.md
  task-schema.md
  task-template.md

runtime/
  active-task.md
  session-notes.md

prompts/
  continue-task.md
  checkpoint-task.md
  placeholder-continue-task.md

docs/
  examples.md
```

Do not load `docs/examples.md` unless examples are requested.

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

`core/problem-understanding.md` requires a short plan plus risks/unknowns before non-trivial or medium/high-impact implementation.

`core/tdd-principles.md` is loaded only when the task changes behavior, fixes a bug, or needs characterization tests before refactoring.

`core/assessment-alignment.md` is loaded only for review/audit tasks to check problem-solving, AI fluency, code quality, communication, and delivery.

## Prompting The Agent Effectively

Some guidance is for the human using the AI extension, not only for the agent. Use this when starting or refining a task.

Provide clear context:

- What you are building and why.
- Constraints and requirements.
- Relevant code snippets or file paths.
- Expected output format.

Ask specific questions:

- Prefer "design a function that does X under constraint Y" over broad prompts.
- Request trade-off discussion when uncertain.
- Ask for alternatives when there are meaningful choices.

Review output critically:

- Read generated code before accepting it.
- Check for errors, edge cases, and style issues.
- Verify it matches your constraints.
- Adjust the prompt and retry if needed.

Never:

- Accept code without reading it.
- Assume the AI understands your domain.
- Skip validation.

## Human Guardrails Checklist

Use this before accepting generated code or ending a task.

Never:

- Accept code without reading it.
- Skip validation.
- Hide failures or debugging.
- Assume AI is correct.
- Over-engineer before validating.

Always:

- Validate early and often.
- Communicate clearly.
- Think before prompting.
- Sanity-check suggestions.
- Iterate with purpose.

### 2. Task Layer

The `tasks/` files define reusable workflows for common work types.

Use one task layer per task:

- `bug-fix.md` for diagnosing and correcting broken behavior
- `business-requirements-planning.md` for converting business requirements into an implementation-ready plan before coding
- `code-review.md` for reviewing branch commits/diffs before applying fixes
- `feature-build.md` for adding new behavior
- `project-bootstrap.md` for scaffolding new projects, apps, services, packages, or monorepos
- `refactor.md` for behavior-preserving structural improvement
- `performance-optimization.md` for latency, throughput, memory, cost, or scalability work
- `risk-discovery.md` for non-functional risk assessment, mitigation strategies, data consistency, and availability review

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

The full placeholder prompt examples are in `docs/examples.md`.

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

Longer continuity prompt examples are in `docs/examples.md`.

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

An example `runtime/active-task.md` is available in `docs/examples.md`.

## Recommended Prompts

Task prompt examples for business requirements planning, bug fixes, feature builds, code reviews, project bootstraps, refactors, performance work, and risk discovery are in `docs/examples.md`.

## How To Choose The Task Type

Choose the task type based on the dominant risk:

- If behavior is wrong, use `bug-fix`.
- If the goal is to turn business requirements into a plan before coding, use `business-requirements-planning`.
- If the goal is to review branch commits or pull-request changes before applying fixes, use `code-review`.
- If new behavior is being added, use `feature-build`.
- If a new project, app, service, package, or monorepo is being created, use `project-bootstrap`.
- If behavior must stay the same while structure improves, use `refactor`.
- If resource usage, latency, throughput, or cost is the main concern, use `performance-optimization`.
- If the goal is to uncover non-functional limitations and follow-up actions, use `risk-discovery`.

When a task overlaps categories, choose the one with the highest operational risk. For example, fixing a production performance regression should use `performance-optimization`, even if it also involves refactoring. Creating a new service inside an existing monorepo should use `project-bootstrap`, then apply feature-build behavior inside that scaffold.

## Project Bootstrap Prompt Examples

Project bootstrap placeholder and filled examples are in `docs/examples.md`.

If `IS_MONOREPO` is `yes`, provide `SERVICES` as a comma-separated or line-separated list.

If `IS_MONOREPO` is `yes` and `SERVICES` is empty, the agent should ask for the service list before creating files.

Set `REQUIRES_PRE_SCAFFOLD_APPROVAL=yes` when you want a decision checkpoint before files are created. The agent should output the proposed projects/services/packages, tool rationale, alternatives, assumptions, risks, and selected capability options, then wait for approval.

Use `NODE_VERSION_POLICY=latest-lts`, `CREATE_NVMRC=yes`, and `SWITCH_NODE_BEFORE_INSTALL=yes` when you want the agent to verify the current active LTS Node version, create `.nvmrc`, switch local Node with available tooling, and only then install dependencies or generate lockfiles.

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
- Update the source-of-truth file first; reference it elsewhere instead of duplicating long instructions.
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
Load only required core files, one task layer, and the active task source.
Do not load README.md, USING-WITH-VSCODE.md, examples, every task file, or optional modules unless needed.
```

Example for a known task:

```text
Use AGENTS.md in low-token mode.
Treat this as a code-review task.
Load only required core files, tasks/code-review.md, and the active task source.
Do not load README.md, USING-WITH-VSCODE.md, examples, every task file, or optional modules unless needed.
```

This keeps the active context small while preserving the same rules.
