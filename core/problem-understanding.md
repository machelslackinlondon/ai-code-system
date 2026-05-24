# Problem Understanding

Use before implementation for non-trivial, medium-impact, or high-impact work.

## Stage 1 - Understand The Problem

- Read the requirement carefully.
- Identify constraints, edge cases, and assumptions.
- Sketch a rough solution approach.
- List uncertainties.
- Identify highest-risk parts.

## Stage 2 - Repo & Context Inspection

Quickly survey:

- Repo structure; do not read everything.
- Existing patterns or conventions.
- Testing setup.
- Build/lint tools.
- Relevant dependencies.

If this is a new repo:

- Spend max 5 minutes.
- Note key findings.
- Move forward.

## Stage 3 - Prompt / Output Discipline

When responding or generating code:

- Use available task context and constraints explicitly.
- State trade-offs when the approach is uncertain.
- Offer alternatives only when they change the decision meaningfully.
- Do not assume missing domain behavior.
- Validate generated changes before presenting them as complete.

## Output

- Brief plan, 3-5 bullets.
- Risks or unknowns.
- Key repo/context findings.
- Trade-offs and alternatives considered.

Do not code until this stage is complete. For tiny low-risk edits, keep this implicit unless the user asks for a plan.
