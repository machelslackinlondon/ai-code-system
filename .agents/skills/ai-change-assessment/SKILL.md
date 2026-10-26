---
name: ai-change-assessment
description: Use when evaluating an AI coding model on a real repository change with independent tests, reproducible trials, or evidence-based scoring.
---

# AI Change Assessment

## Overview

The evaluator owns the change contract, independent tests, and verdict; the model owns only its submitted solution.

**Core principle:** Freeze the evaluation before the run and score the untouched submission from reproducible evidence.

## Protocol

1. Select a small, behavior-visible change. Record the revision, acceptance criteria, constraints, non-goals, tools, commands, limits, trials, retry policy, rubric, and outcome rules. Avoid external services, migrations, broad refactors, and undocumented product decisions.
2. Inspect relevant code and run targeted checks to capture pre-existing failures.
3. Write evaluator-owned tests before the model run. Confirm new behavior tests fail for the intended reason. Mark tests visible or hidden; keep verified evaluator tests outside the model's control.
4. Freeze the exact prompt. Include behavior and constraints, but not hidden tests, scoring internals, an expected patch, or solution hints.
5. Start every trial from the same clean revision. Record platform, model/version, system instructions, settings, tools, transcript, duration, and interventions; unknown settings stay `unknown`. A pre-submission infrastructure retry is void. A new solution attempt is a separate trial without earlier-result feedback unless predefined.
6. Preserve the final patch without repair. If the platform is unavailable, produce a run packet and wait for the transcript and patch; never impersonate the evaluated model.
7. Run evaluator tests, model-authored tests, existing tests, then lint/type-check/build. Inspect correctness, regression, tampering, security, maintainability, and scope. Changes to evaluator tests are tampering; separate new tests are model-authored coverage.
8. Score recorded evidence. Report first-attempt and best-of-N results separately. Disqualified trials are non-passes; inconclusive trials are reported but excluded from the pass-rate denominator.

## Default Rubric

| Dimension | Weight |
| --- | ---: |
| Acceptance criteria and evaluator-test correctness | 45 |
| Regression safety | 15 |
| Implementation quality and repository fit | 15 |
| Model-authored test quality | 10 |
| Scope and constraint discipline | 10 |
| Autonomy and validation reporting | 5 |

Freeze weight changes before running. Verdicts: 90–100 strong pass; 75–89 pass; 60–74 mixed; below 60 fail. Core-behavior or regression failures cap the score at 59. Tampering, concealed failures, or destructive behavior may disqualify.

## Prompt Contract

```text
Implement: <one specific repository change>
Acceptance criteria: <observable outcomes>
Constraints: preserve unrelated behavior; follow conventions; avoid unrelated refactors; do not weaken validation.
Validation: run <visible commands>; report files, results, failures, and risks.
```

## Assessment Output

Return metadata, contract, exact prompt, trials/settings, test and validation evidence, diff findings, rubric scores, verdict, interventions, limitations, and artifact locations. Hash key artifacts when possible; redact secrets and state retention limits.

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| Post-hoc tests or patch repairs | Label them separately; score the frozen tests and original patch. |
| Visible tests treated as sufficient | Run independent tests and inspect behavior and scope. |
| Selective retries or guessed defaults | Predefine retries; record unavailable settings as `unknown`. |
