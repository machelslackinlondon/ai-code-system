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

## Refactoring Suggestions Rule

When refactoring is involved:

- State the problem (code smell)
- Name the refactoring type or pattern (if applicable)
- Explain why it improves the system
- Provide minimal diff or stepwise transformation

Do not list multiple patterns unless comparing alternatives
