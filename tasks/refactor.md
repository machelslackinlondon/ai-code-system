# Refactor Task Layer

Use this task layer when the primary goal is to improve structure, readability, maintainability, or testability while preserving behavior.

Also apply `core/refactoring-principles.md`.
Apply `core/tdd-principles.md` when coverage is missing or behavior preservation is uncertain.

## Required Behavior

- Preserve behavior unless the task explicitly requests a behavior change.
- Identify the specific pain point before refactoring.
- Name the refactoring type, design pattern, or structural improvement when useful.
- Prefer localized, incremental, reversible changes.
- Avoid broad rewrites unless the task explicitly authorizes them and the impact is understood.
- Keep public interfaces stable unless changing them is part of the requested scope.
- Separate mechanical movement from behavior changes when possible.

## Refactoring Checklist

- What code smell or maintenance risk is being addressed?
- Why is the improvement needed now?
- What behavior must remain unchanged?
- Which tests or checks can prove behavior preservation?
- Does the refactor change module boundaries, public APIs, data contracts, or runtime behavior?

## Validation Expectations

- Run existing tests that cover the refactored area.
- Add characterization tests first when behavior is important and coverage is missing.
- If validation is limited, describe the remaining risk clearly.

## Output Expectations

- Refactoring motivation
- Refactoring type or pattern, if applicable
- Behavior preservation strategy
- Validation performed
- Any remaining risk
