# Checkpoint Task Prompt

Use this prompt when you want the agent to save the current context without making further code changes.

```text
Use AGENTS.md.

Create or update runtime/session-notes.md as a context checkpoint for the current task.

Capture:

- current goal
- active task source
- decisions made
- files changed
- validation run
- open questions
- open items
- risks or constraints
- what the next prompt should do

Do not make unrelated code changes.
Keep the notes concise enough to be useful in the next session.
```

