# Create Jira Ticket Task Layer

Use this task layer when the requested outcome is to create, draft, or verify creation of a Jira ticket/work item through a Jira MCP server.

## Required Behavior

- Use the configured Jira MCP server tools for Jira reads and writes. Do not bypass MCP with direct REST calls unless the user explicitly asks for that fallback.
- Never hardcode Jira credentials, API tokens, OAuth secrets, or personal account details in repository files, prompts, logs, or session notes.
- Require `JIRA_PROJECT_KEY`, `JIRA_ISSUE_TYPE`, `JIRA_SUMMARY`, and `JIRA_DESCRIPTION` before creating an issue. If any are missing, produce a concise draft payload and ask only for the missing values.
- If `JIRA_CREATE_MODE` is missing, treat it as `auto`. In `auto` mode, create exactly one Jira issue after required fields are validated and no duplicate is found.
- If `JIRA_CREATE_MODE=draft`, do not create an issue; output the Jira payload that would be created.
- If `JIRA_CREATE_MODE=approval-required`, present the exact Jira payload and wait for explicit approval before invoking the create tool.
- Search for an existing issue before creation using `JIRA_DEDUPLICATION_JQL` when provided; otherwise search by project key and exact or near-exact summary. If a likely duplicate exists, do not create a new issue unless the user explicitly confirms.
- Verify project, issue type, and required fields with Jira MCP tools when those discovery tools are available. If Jira rejects the create request, inspect the structured error, fix the payload when safe, and retry at most once.
- Create bulk tickets only when the task explicitly provides multiple independent ticket payloads or an exact requested count.
- After successful creation, report the issue key and URL, and update `runtime/session-notes.md` when continuity matters.

## Task Checklist

- Which Jira site/account is the MCP server connected to?
- What project key and issue type should own the ticket?
- Is the summary short, specific, and user-facing?
- Does the description include enough context, expected outcome, acceptance criteria, and source references?
- Are labels, components, parent/epic, assignee, priority, and due date needed or intentionally omitted?
- What deduplication query or search proves this ticket should be new?
- What validation proves the created ticket matches the requested work?

## Constraints

- Keep Jira payloads minimal and aligned with fields the target Jira project accepts.
- Do not infer security-sensitive values, customer data, severity, assignee, sprint, or due date unless provided by the task context or discoverable through Jira MCP metadata.
- Do not create subtasks unless `JIRA_PARENT` is provided and validated.
- Do not mark Jira ticket creation as complete without either a created issue key/URL or a clear blocker explaining why creation could not happen.

## Validation Expectations

- Confirm the Jira MCP server/tool is available before attempting creation.
- Validate required fields and deduplication search before the create call.
- Verify the created issue by reading it back through Jira MCP when available.
- If validation cannot be run, explain why and provide the best available draft payload.

## Output Expectations

- Jira action taken: created, duplicate found, draft only, awaiting approval, or blocked.
- Created or existing issue key and URL when available.
- Final Jira payload summary.
- Validation performed.
- Residual risks or missing fields only when they materially affect ticket quality.
