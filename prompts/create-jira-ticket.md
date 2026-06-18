# Create Jira Ticket Prompt

```text
Use AGENTS.md.
Select `tasks/create-jira-ticket.md`.
Use placeholders from this prompt as the active task when provided; otherwise use `runtime/active-task.md`.

TASK_NAME={{TASK_NAME}}
TASK_TYPE=create-jira-ticket
TASK_DESCRIPTION={{TASK_DESCRIPTION}}
IMPACT_LEVEL={{IMPACT_LEVEL}}
AFFECTED_SYSTEMS={{AFFECTED_SYSTEMS}}
RELATED_FILES={{RELATED_FILES}}
CONSTRAINTS={{CONSTRAINTS}}
NON_GOALS={{NON_GOALS}}
INPUTS={{INPUTS}}
EXPECTED_OUTPUT={{EXPECTED_OUTPUT}}
VALIDATION_PLAN={{VALIDATION_PLAN}}
ROLLBACK_PLAN={{ROLLBACK_PLAN}}
TDD_MODE={{TDD_MODE}}

JIRA_SITE={{JIRA_SITE}}
JIRA_PROJECT_KEY={{JIRA_PROJECT_KEY}}
JIRA_ISSUE_TYPE={{JIRA_ISSUE_TYPE}}
JIRA_SUMMARY={{JIRA_SUMMARY}}
JIRA_DESCRIPTION={{JIRA_DESCRIPTION}}
JIRA_PRIORITY={{JIRA_PRIORITY}}
JIRA_LABELS={{JIRA_LABELS}}
JIRA_COMPONENTS={{JIRA_COMPONENTS}}
JIRA_ASSIGNEE={{JIRA_ASSIGNEE}}
JIRA_PARENT={{JIRA_PARENT}}
JIRA_ACCEPTANCE_CRITERIA={{JIRA_ACCEPTANCE_CRITERIA}}
JIRA_DUE_DATE={{JIRA_DUE_DATE}}
JIRA_CREATE_MODE={{JIRA_CREATE_MODE}}
JIRA_DEDUPLICATION_JQL={{JIRA_DEDUPLICATION_JQL}}

Before creating, validate required Jira fields and search for duplicates.
If `JIRA_CREATE_MODE` is omitted, treat it as `auto`.
If Jira MCP tools, authentication, or required fields are unavailable, do not create a Jira issue; report the blocker and provide a draft payload.
After successful creation, update `runtime/session-notes.md` with the created issue key, URL, validation performed, residual risks, and next step.
```
