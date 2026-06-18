# Jira Ticket Automation README

This guide explains how the Jira ticket creation task works in this prompt system and how to automate ticket creation through the configured Jira MCP server.

The source of truth for agent behavior is `tasks/create-jira-ticket.md`. This document is an operator guide for setting up and using that task.

## What This Enables

Use the `create-jira-ticket` task when an agent should create, draft, or verify a Jira issue from structured task fields.

The flow supports:

- Creating exactly one Jira issue automatically when required fields are present.
- Drafting a Jira payload without creating anything.
- Requiring approval before creation.
- Searching for duplicates before creation.
- Recording the created Jira issue key and URL in `runtime/session-notes.md`.

## Required Setup

1. Configure the Jira MCP server in `.vscode/mcp.json`.
2. Start and trust the `jira` MCP server in VS Code.
3. Provide the Jira MCP server URL when VS Code prompts for `jira-mcp-url`.
4. Ensure the connected Jira account has permission to browse projects and create issues in the target project.

The workspace MCP config is intentionally credential-free:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "jira-mcp-url",
      "description": "Jira MCP server URL"
    }
  ],
  "servers": {
    "jira": {
      "type": "http",
      "url": "${input:jira-mcp-url}"
    }
  }
}
```

Do not commit Jira tokens, OAuth secrets, personal account values, or customer-sensitive ticket data.

## Required Ticket Fields

The agent must have these fields before it can create a Jira issue:

- `JIRA_PROJECT_KEY`
- `JIRA_ISSUE_TYPE`
- `JIRA_SUMMARY`
- `JIRA_DESCRIPTION`

Optional fields:

- `JIRA_SITE`
- `JIRA_PRIORITY`
- `JIRA_LABELS`
- `JIRA_COMPONENTS`
- `JIRA_ASSIGNEE`
- `JIRA_PARENT`
- `JIRA_ACCEPTANCE_CRITERIA`
- `JIRA_DUE_DATE`
- `JIRA_DEDUPLICATION_JQL`

After creation, the agent can record:

- `JIRA_CREATED_ISSUE_KEY`
- `JIRA_CREATED_ISSUE_URL`

## Create Modes

`JIRA_CREATE_MODE` controls whether the agent creates the issue or only prepares it.

| Mode | Behavior |
| --- | --- |
| `auto` | Default. Validate fields, search for duplicates, then create one Jira issue if safe. |
| `approval-required` | Show the exact Jira payload and wait for explicit user approval before creating. |
| `draft` | Do not create an issue. Output the payload that would be created. |

Use `approval-required` for sensitive projects, production incidents, customer-facing tickets, or any workflow where accidental ticket creation is costly.

## Automation Flow

When `JIRA_CREATE_MODE=auto` or the field is omitted, the agent should:

1. Load `AGENTS.md` and select `tasks/create-jira-ticket.md`.
2. Confirm the Jira MCP server tools are available.
3. Validate required Jira fields.
4. Search for an existing issue using `JIRA_DEDUPLICATION_JQL` when present.
5. If no duplicate is found, create exactly one Jira issue through Jira MCP.
6. Read the issue back through Jira MCP when possible.
7. Report the created issue key and URL.
8. Update `runtime/session-notes.md` when continuity matters.

If the MCP server, authentication, required fields, or Jira project metadata are unavailable, the agent must not create the issue. It should report the blocker and provide a draft payload instead.

## Prompt Usage

Use the dedicated prompt template:

```text
Use prompts/create-jira-ticket.md.

TASK_NAME=Create Jira ticket for login retry handling
TASK_DESCRIPTION=Create a Jira issue for adding retry handling around login API timeouts.
IMPACT_LEVEL=medium
AFFECTED_SYSTEMS=login API, frontend authentication flow
EXPECTED_OUTPUT=Created Jira issue key and URL

JIRA_PROJECT_KEY=ENG
JIRA_ISSUE_TYPE=Task
JIRA_SUMMARY=Add retry handling for login API timeouts
JIRA_DESCRIPTION=Users may see transient login failures when the login API times out. Add bounded retry handling and clear error messaging. Include tests for timeout and final failure behavior.
JIRA_ACCEPTANCE_CRITERIA=- Login retries are bounded and observable
- Timeout failure message is clear
- Tests cover retry success and retry exhaustion
JIRA_CREATE_MODE=auto
JIRA_DEDUPLICATION_JQL=project = ENG AND summary ~ "login API timeouts" ORDER BY created DESC
```

For saved-prompt or placeholder workflows, pass the same `JIRA_*` fields through `prompts/placeholder-continue-task.md`.

## Draft-Only Example

Use draft mode when planning work before Jira is connected:

```text
Use prompts/create-jira-ticket.md.

TASK_NAME=Draft Jira ticket for audit logging
TASK_TYPE=create-jira-ticket
JIRA_PROJECT_KEY=PLAT
JIRA_ISSUE_TYPE=Story
JIRA_SUMMARY=Add audit logging for admin role changes
JIRA_DESCRIPTION=Create audit records whenever an admin grants or revokes a role. Include actor, target user, role, timestamp, and request id.
JIRA_CREATE_MODE=draft
```

The agent should return the payload only and must not create a Jira issue.

## Safe Automation Rules

- Never bypass the Jira MCP server with direct REST calls unless explicitly requested.
- Never create a ticket if required fields are missing.
- Never create a duplicate when a likely existing issue is found.
- Never infer sensitive values such as customer names, assignee, sprint, severity, or due date unless supplied or safely discoverable.
- Retry a failed Jira create request at most once, and only after correcting a safe payload issue.
- For bulk ticket creation, require explicit multiple payloads or an exact requested count.

## Troubleshooting

| Symptom | Likely Cause | Action |
| --- | --- | --- |
| Agent returns a draft instead of creating | Jira MCP server unavailable or auth missing | Start/trust the `jira` MCP server and complete auth. |
| Jira rejects the payload | Required project fields differ by Jira project | Ask the agent to inspect project metadata through Jira MCP and retry once. |
| Duplicate found | Deduplication search matched an existing issue | Use the existing issue or explicitly confirm that a new issue should be created. |
| No issue key or URL returned | Create verification failed | Ask the agent to read back the created issue through Jira MCP or report the blocker. |

## Files Involved

- `AGENTS.md`: lists `tasks/create-jira-ticket.md` as a selectable task layer.
- `tasks/create-jira-ticket.md`: source-of-truth behavior for Jira ticket creation.
- `tasks/task-schema.md`: defines Jira task fields.
- `runtime/active-task.md`: includes Jira fields for repo-stored active tasks.
- `prompts/create-jira-ticket.md`: dedicated prompt template for Jira ticket creation.
- `prompts/placeholder-continue-task.md`: placeholder support for saved-prompt workflows.
- `.vscode/mcp.json`: workspace Jira MCP server configuration.
