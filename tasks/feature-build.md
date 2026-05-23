# Feature Build Task Layer

Use this task layer when adding new behavior, capability, endpoint, UI, workflow, configuration support, or integration.

## Required Behavior

- Clarify the intended user or system outcome before implementation.
- Inspect existing patterns before adding new abstractions, dependencies, or conventions.
- Keep the first implementation scoped to the requested feature.
- Preserve existing behavior unless the task explicitly requires a behavior change.
- Avoid opportunistic refactors unless they are required for correctness, safety, or a clean integration point.
- Consider compatibility, rollout, observability, failure modes, and testability.
- Use feature flags, configuration, or staged rollout patterns when the repository already uses them and the impact warrants it.

## Design Checklist

- What existing module, boundary, or API should own this behavior?
- What data contracts, validation rules, or permissions apply?
- What errors, empty states, or degraded states should be handled?
- What tests or checks prove the feature works without regressing existing behavior?
- Does the feature introduce latency, cost, migration, or operational risk?

## Validation Expectations

- Add or update tests around new behavior when a test framework exists.
- Validate important edge cases, not only the happy path.
- For UI changes, verify responsive and interaction states when feasible.
- For API or backend changes, validate input handling, error paths, and compatibility.

## Output Expectations

- Feature summary
- Files changed
- Validation performed
- Important edge cases covered
- Follow-up risks only when they materially affect delivery
