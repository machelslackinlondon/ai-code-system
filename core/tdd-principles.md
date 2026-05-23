# TDD Principles

Use for behavior changes, bug fixes, and behavior-preserving refactors with weak coverage.

## Flow

1. Define expected behavior.
2. Add or update the smallest meaningful failing test.
3. Confirm it fails for the expected reason.
4. Implement the smallest production change.
5. Make the test pass.
6. Refactor only after green.
7. Run targeted regression checks.

## Rules

- Prefer tests closest to the changed behavior.
- Bug fixes should add regression coverage when feasible.
- Features should cover happy path plus important edge/error cases.
- Refactors should use existing or characterization tests to preserve behavior.
- If tests are impractical, explain why and use the best available validation.
