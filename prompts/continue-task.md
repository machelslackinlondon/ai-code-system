# Continue Task Prompt

Use this prompt at the start of a follow-up session.

```text
Use AGENTS.md.

Continue from:

- runtime/active-task.md
- runtime/session-notes.md

Preserve the existing context.
Do not restart the task from scratch unless the current request explicitly changes direction.

Before changing files:

1. Read AGENTS.md.
2. Read runtime/active-task.md.
3. Read runtime/session-notes.md.
4. Identify the relevant task layer from tasks/.
5. Continue from the open items and next steps.

Before finishing:

1. Update runtime/session-notes.md with decisions made, files changed, validation run, open questions, open items, and next steps.
2. Summarize what changed and what remains.
```

