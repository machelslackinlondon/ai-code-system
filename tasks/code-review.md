# Code Review Task Layer

Use when reviewing branch commits, diffs, or pull-request changes in the current repository.

Default behavior is review-only. Do not modify files unless the user explicitly approves a proposed fix.

## Required Inputs

- `REVIEW_BASE_REF`: base branch, tag, or commit
- `REVIEW_HEAD_REF`: head branch or commit; default current `HEAD`
- `REVIEW_RANGE`: explicit commit range when provided; otherwise derive the full branch range from the merge-base/first divergent commit
- `REVIEW_REQUIRE_TESTS`: yes, no, or auto
- `REVIEW_APPLY_FIXES`: no by default

If no base ref or range is provided, infer the most likely base branch from repository context when safe; otherwise ask.

## Range Selection

By default, review all changes introduced on the branch from the first commit that diverged from the base branch through `REVIEW_HEAD_REF`.

Use this precedence:

1. If `REVIEW_RANGE` is provided, review that exact range.
2. Else if `REVIEW_BASE_REF` and `REVIEW_HEAD_REF` are provided, find their merge-base and review `merge-base..REVIEW_HEAD_REF`.
3. Else infer the likely base branch, find `merge-base inferred-base REVIEW_HEAD_REF`, and review `merge-base..REVIEW_HEAD_REF`.
4. If the base branch cannot be inferred safely, ask before reviewing.

Do not review only the latest commit unless `REVIEW_RANGE` explicitly asks for it.

## Review Scope

Review introduced changes for:

- Correctness and behavioral regressions
- Scalability and concurrency risks
- Maintainability and readability
- Security, authentication, authorization, secrets, and dependency risk
- Performance, latency, memory, and cost
- Data consistency and availability
- Test coverage and passing unit tests
- Build/lint/typecheck impact
- Observability, rollout, and rollback concerns

## Required Behavior

- Inspect only the current repository/workspace unless the user provides external context.
- Review commits/diffs relevant to the selected range, including every branch commit and the combined diff from the first divergent commit through `REVIEW_HEAD_REF`.
- Prioritize findings by severity and likelihood.
- Provide evidence with file paths, changed behavior, failing checks, or commit references.
- Distinguish confirmed issues from assumptions or unknowns.
- Run or inspect unit tests when feasible; if not run, explain why.
- Classify risk introduced by the change as `low`, `medium`, or `high`.
- Explain each proposed fix before applying it.
- Ask for approval before writing or applying fixes.

## Risk Classification

- Low: isolated change, low blast radius, tests/validation cover the behavior.
- Medium: behavior/config change, partial test coverage, moderate operational or data risk.
- High: auth, security, data model, migration, infrastructure, critical path, availability, or performance-critical change.

## Output Expectations

Lead with findings:

| Severity | Area | Finding | Evidence | Risk | Recommendation |
| --- | --- | --- | --- | --- | --- |

Then include:

- Review range
- Summary of changed behavior
- Unit test/build/lint status
- Introduced risk assessment: low, medium, or high
- Security/performance/scalability/maintainability notes
- Data consistency and availability notes
- Proposed fixes requiring approval
- Open questions or unknowns

If there are no findings, say so clearly and list residual test gaps or risks.
