# Output Format Rules

## Impact Classification

Classify all changes as:

- Low impact: isolated code changes, no system behavior change
- Medium impact: logic changes, configuration changes, non-critical flows
- High impact: infrastructure, authentication, data models, performance-critical paths

## Impact & Trade-off Summary (Required for Medium/High Impact)

For medium/high-impact changes, produce this BEFORE implementation:

### Impact Summary Template

- Change description:
- Systems affected:
- Consistency vs availability impact:
- Latency / performance impact:
- Infra / Terraform implications:
- Failure modes introduced:
- Rollback strategy:
- Reason for chosen approach:

## Execution Flow Rules

For all changes:

1. Validate relevant repository context before editing
2. Classify impact level
3. Choose the smallest safe implementation path
4. Validate the changed behavior when feasible

For medium/high-impact changes:

1. Produce Impact & Trade-off Summary
2. Pause before implementation (or wait for confirmation if interactive)
3. Only then proceed with implementation

Do not interleave analysis and implementation for high-impact changes.

## Stage 4 - Validation Loop

For each change:

1. Explain what is being implemented and why
2. Implement the smallest useful increment
3. Run immediate validation: build, test, lint, typecheck, or targeted manual check
4. Inspect output and errors
5. If broken, diagnose and iterate
6. If working, move to the next increment

Prefer small increments, frequent validation, and early error detection.

## Stage 5 - Quality Gates

Before committing or presenting changes as complete:

### Does It Work?

- Builds or compiles when applicable
- Basic tests pass when available
- No obvious bugs remain

### Is It Readable?

- Code is clear and concise
- Edge cases are handled
- Assumptions are documented where useful

### Can You Defend It?

- You understand every changed line
- You can explain why this approach was chosen
- You can explain relevant trade-offs

## Stage 6 - Communication

As you work, explain:

- What problem you are solving
- Why you chose the approach, including trade-offs
- What you validated and how
- Surprises or blockers encountered
- What you are iterating on and why

Narrate when useful:

- Debugging steps
- Why an AI suggestion was accepted or refined
- Assumptions being made

Keep communication concise. Do not narrate obvious mechanical steps unless they affect risk, correctness, or user decisions. Act like a developer shipping real code under time pressure.

## Stage 7 - Iteration With Purpose

If something does not work:

1. Diagnose the root cause; do not guess
2. Explain the failure clearly
3. Adjust the approach, code, or AI prompt
4. Validate again
5. Note what was learned

Use AI for design discussion when useful:

- Ask about trade-offs
- Request alternative approaches
- Refine based on feedback

Do not keep retrying the same approach without new evidence or a changed hypothesis.

## Response Format

### Medium / High Impact Changes

- Plan (brief)
- Implementation (diff or code)
- Risks / Trade-offs (only if relevant)
- Validation performed

### Low Impact Changes

- Validate relevant context first
- Implement directly
- Run targeted validation when feasible
- Keep explanation minimal unless requested

For completed work, summarize validation run, quality gates not run and why, and residual risks.

## Refactoring Suggestions Rule

When refactoring is involved:

- State the problem (code smell)
- Name the refactoring type or pattern (if applicable)
- Explain why it improves the system
- Provide minimal diff or stepwise transformation

Do not list multiple patterns unless comparing alternatives
