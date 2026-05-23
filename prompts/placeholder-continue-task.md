# Placeholder Continue Task Prompt

Use this prompt when your VS Code AI extension stores task details in prompt placeholders instead of `runtime/active-task.md`.

```text
Use AGENTS.md.

Use the placeholder values below as the active task for this session.
Use runtime/session-notes.md as continuity context.

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

SESSION_CONTEXT:
{{SESSION_CONTEXT}}

RECENT_DECISIONS:
{{RECENT_DECISIONS}}

OPEN_ITEMS:
{{OPEN_ITEMS}}

Project bootstrap fields, if applicable:

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

Before finishing, update runtime/session-notes.md with the new checkpoint unless the user asks not to write files.
```

