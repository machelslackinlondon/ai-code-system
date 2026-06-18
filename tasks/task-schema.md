# Task Schema

Use this schema for `runtime/active-task.md` and concrete task descriptions. Use `tasks/task-template.md` when creating a new reusable task archetype.

## Required Fields

TASK_NAME:

TASK_TYPE:

- bug-fix
- business-requirements-planning
- code-review
- create-jira-ticket
- feature-build
- project-bootstrap
- refactor
- performance-optimization
- risk-discovery
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

NODE_VERSION_POLICY:

- latest-lts
- explicit
- repo-existing
- unknown

NODE_VERSION:

CREATE_NVMRC:

- yes
- no
- auto

SWITCH_NODE_BEFORE_INSTALL:

- yes
- no
- auto

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

## Risk Discovery Fields

Use these fields when `TASK_TYPE` is `risk-discovery`.

RISK_AREAS:

RISK_SEVERITY_THRESHOLD:

- low
- medium
- high
- all

INCLUDE_MITIGATION_STRATEGIES:

- yes
- no

INCLUDE_DATA_CONSISTENCY_ANALYSIS:

- yes
- no

INCLUDE_AVAILABILITY_ANALYSIS:

- yes
- no

INCLUDE_DESIGN_FOR_FAILURE_REVIEW:

- yes
- no

## Business Requirements Planning Fields

Use these fields when `TASK_TYPE` is `business-requirements-planning`.

BUSINESS_GOAL:

TARGET_USERS:

SUCCESS_METRICS:

USER_OUTCOMES:

BUSINESS_CONSTRAINTS:

PLANNING_DEPTH:

- brief
- standard
- detailed

REQUIRES_IMPLEMENTATION_APPROVAL:

- yes
- no
- auto

## Code Review Fields

Use these fields when `TASK_TYPE` is `code-review`.

REVIEW_BASE_REF:

REVIEW_HEAD_REF:

REVIEW_RANGE:

REVIEW_RANGE_MODE:

- full-branch
- explicit-range
- latest-commit

REVIEW_REQUIRE_TESTS:

- yes
- no
- auto

REVIEW_APPLY_FIXES:

- yes
- no

REVIEW_FOCUS_AREAS:

REVIEW_RISK_THRESHOLD:

- low
- medium
- high
- all

## Jira Ticket Fields

Use these fields when `TASK_TYPE` is `create-jira-ticket`.

JIRA_SITE:

JIRA_PROJECT_KEY:

JIRA_ISSUE_TYPE:

JIRA_SUMMARY:

JIRA_DESCRIPTION:

JIRA_PRIORITY:

JIRA_LABELS:

JIRA_COMPONENTS:

JIRA_ASSIGNEE:

JIRA_PARENT:

JIRA_ACCEPTANCE_CRITERIA:

JIRA_DUE_DATE:

JIRA_CREATE_MODE:

- auto
- approval-required
- draft

JIRA_DEDUPLICATION_JQL:

JIRA_CREATED_ISSUE_KEY:

JIRA_CREATED_ISSUE_URL:

## Field Rules

- `TASK_DESCRIPTION` must describe the desired outcome, not only the implementation idea.
- `IMPACT_LEVEL` must follow `core/output-format.md`.
- `AFFECTED_SYSTEMS` should include services, modules, databases, APIs, queues, jobs, infrastructure, or user-facing flows.
- `RELATED_FILES` may be empty at task start, but the agent should update its working context by inspecting the repository.
- `CONSTRAINTS` should include compatibility, security, data, latency, rollout, style, or dependency limits.
- `NON_GOALS` prevents unrelated expansion.
- For `create-jira-ticket`, `JIRA_PROJECT_KEY`, `JIRA_ISSUE_TYPE`, `JIRA_SUMMARY`, and `JIRA_DESCRIPTION` are required before creating an issue.
- For `create-jira-ticket`, `JIRA_CREATE_MODE` defaults to `auto` when omitted.
- `VALIDATION_PLAN` should name tests, checks, manual verification, or the reason validation is not available.
- `ROLLBACK_PLAN` is required for medium/high-impact changes.
- `TDD_MODE` defaults to `auto`. Use `required` to force red-green-refactor when feasible, or `skip` only when tests are impractical or out of scope.
- For `project-bootstrap`, `IS_MONOREPO` must be known before scaffolding. If it is `yes`, `SERVICES` must list each service/package to create.
- For `project-bootstrap`, use `tasks/project-bootstrap.md` as the source of truth for default stack and scaffold expectations.
- For `project-bootstrap`, `NODE_VERSION_POLICY` defaults to `latest-lts`; verify the current active LTS version at scaffold time before creating `.nvmrc`.
- For `project-bootstrap`, `CREATE_NVMRC` defaults to `yes`, and `SWITCH_NODE_BEFORE_INSTALL` defaults to `yes` when local tooling supports it.
- For `project-bootstrap`, `REQUIRES_PRE_SCAFFOLD_APPROVAL` defaults to `auto`; treat `auto` as `yes` for monorepos and medium/high-impact scaffolds.
- For `project-bootstrap`, `INCLUDE_*` fields default to `auto`; use task description and constraints to decide, then explain included/skipped options in the pre-scaffold proposal.
- For `risk-discovery`, do not implement fixes unless explicitly requested.
- For `risk-discovery`, default `INCLUDE_MITIGATION_STRATEGIES`, `INCLUDE_DATA_CONSISTENCY_ANALYSIS`, `INCLUDE_AVAILABILITY_ANALYSIS`, and `INCLUDE_DESIGN_FOR_FAILURE_REVIEW` to `yes`.
- For `business-requirements-planning`, default `PLANNING_DEPTH` to `standard` and `REQUIRES_IMPLEMENTATION_APPROVAL` to `yes`.
- For `business-requirements-planning`, produce a plan and recommended next task type before implementation.
- For `code-review`, default `REVIEW_HEAD_REF` to current `HEAD`, `REVIEW_RANGE_MODE` to `full-branch`, `REVIEW_REQUIRE_TESTS` to `auto`, and `REVIEW_APPLY_FIXES` to `no`.
- For `code-review`, when `REVIEW_RANGE_MODE` is `full-branch`, review all branch changes from the merge-base/first divergent commit through `REVIEW_HEAD_REF`.
- For `code-review`, ask for approval before writing or applying fixes.
