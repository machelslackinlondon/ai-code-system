# Codex IDE Prompt System

Portable "Core Rules + Task Layer" instructions for AI coding agents.

## Load Order

Load only what is needed:

1. Always: `core/rules.md`, `core/engineering-principles.md`, `core/output-format.md`, `core/problem-understanding.md`
2. Conditional:
   - TDD/behavior changes: `core/tdd-principles.md`
   - Refactor work: `core/refactoring-principles.md`
   - Architecture/design pattern decisions: `core/design-patterns.md`
   - Review/audit tasks: `core/assessment-alignment.md`
3. One task layer:
   - `tasks/bug-fix.md`
   - `tasks/business-requirements-planning.md`
   - `tasks/code-review.md`
   - `tasks/create-jira-ticket.md`
   - `tasks/feature-build.md`
   - `tasks/project-bootstrap.md`
   - `tasks/refactor.md`
   - `tasks/performance-optimization.md`
   - `tasks/risk-discovery.md`
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
- Do not load `docs/examples.md` unless examples are requested.
- Do not load every task file; load only the selected task layer.
- Do not load optional core modules unless relevant.
- Keep `runtime/session-notes.md` concise: decisions, changed files, validation, open items, risks, next step.
- Summarize context instead of copying long file contents into responses.
- Prefer links/paths over repeated pasted instructions.

## Maintenance Rule

When updating this prompt system, update the source-of-truth file first. Other docs should reference that file instead of duplicating long instructions.

## Active Task Fields

Standard fields: `TASK_NAME`, `TASK_TYPE`, `TASK_DESCRIPTION`, `IMPACT_LEVEL`, `AFFECTED_SYSTEMS`, `RELATED_FILES`, `CONSTRAINTS`, `NON_GOALS`, `INPUTS`, `EXPECTED_OUTPUT`, `VALIDATION_PLAN`, `ROLLBACK_PLAN`, `TDD_MODE`.

Jira ticket fields: `JIRA_SITE`, `JIRA_PROJECT_KEY`, `JIRA_ISSUE_TYPE`, `JIRA_SUMMARY`, `JIRA_DESCRIPTION`, `JIRA_PRIORITY`, `JIRA_LABELS`, `JIRA_COMPONENTS`, `JIRA_ASSIGNEE`, `JIRA_PARENT`, `JIRA_ACCEPTANCE_CRITERIA`, `JIRA_DUE_DATE`, `JIRA_CREATE_MODE`, `JIRA_DEDUPLICATION_JQL`, `JIRA_CREATED_ISSUE_KEY`, `JIRA_CREATED_ISSUE_URL`.

Project bootstrap fields: `IS_MONOREPO`, `SERVICES`, `APP_NAME`, `SQL_DATABASE`, `DEPLOYMENT_TARGET`, `DEFAULT_STACK`, `SCAFFOLD_REQUIREMENTS`.

## Agent Behavior

- Validate repository context before changing files.
- For non-trivial or medium/high-impact work, complete problem understanding before coding.
- Classify impact before implementation.
- Use prioritisation rules from `core/engineering-principles.md` when sequencing work.
- Use the smallest safe change.
- Use the validation loop from `core/output-format.md` during implementation.
- Apply quality gates from `core/output-format.md` before committing or presenting work as complete.
- Use Stage 6 communication rules from `core/output-format.md` while working.
- Use Stage 7 iteration rules from `core/output-format.md` when validation fails or the approach changes.
- Avoid unrelated refactors.
- Follow repository conventions.
- Run targeted validation when feasible.
- Use TDD for bug fixes, behavior-changing features, and refactors with weak coverage when feasible.
- Report changes, validation, and residual risk.
- Update `runtime/session-notes.md` before finishing when continuity matters.

## Task Selection

- `bug-fix.md`: broken or unexpected behavior.
- `business-requirements-planning.md`: convert business requirements into an implementation-ready plan before coding.
- `code-review.md`: review branch commits/diffs for correctness, risk, tests, security, performance, scalability, and maintainability.
- `create-jira-ticket.md`: create, draft, or verify Jira ticket creation through the configured Jira MCP server.
- `feature-build.md`: new behavior.
- `project-bootstrap.md`: new project, app, service, package, or monorepo.
- `refactor.md`: behavior-preserving structure change.
- `performance-optimization.md`: latency, throughput, memory, cost, or scalability.
- `risk-discovery.md`: inspect non-functional limitations and produce a risk register with mitigations.

For project bootstrap defaults, use `tasks/project-bootstrap.md` as source of truth. If `IS_MONOREPO=yes` and `SERVICES` is missing, ask before scaffolding.
