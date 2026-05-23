# Bug Fix Task Layer

Use this task layer when the requested work is to diagnose and correct broken, incorrect, flaky, or unexpected behavior.

## Required Behavior

- Identify the observed failure before changing code when feasible.
- Reproduce the issue or locate the failing path using tests, logs, code inspection, or a minimal scenario.
- Prefer the smallest change that addresses the root cause.
- Avoid unrelated refactors, formatting churn, or broad rewrites.
- Preserve public behavior except for the bug being fixed.
- Add or update regression coverage when practical.
- Consider whether the bug can affect data integrity, security, availability, or user-visible correctness.

## Investigation Checklist

- What is the expected behavior?
- What is the actual behavior?
- What input, state, or environment triggers the issue?
- Is the failure deterministic, intermittent, or environment-specific?
- Which module owns the failing behavior?
- Are there adjacent edge cases that should be validated?

## Validation Expectations

- Run the most targeted relevant test first.
- Add regression coverage for non-trivial fixes when the repository has a suitable test pattern.
- If tests cannot be run, explain why and provide the best available static or manual validation.

## Output Expectations

- Root cause
- Fix summary
- Validation performed
- Residual risk, if any
