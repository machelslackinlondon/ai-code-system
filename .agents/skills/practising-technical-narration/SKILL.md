---
name: practising-technical-narration
description: Use when preparing for, rehearsing, or reviewing a screen-recorded and voice-narrated technical workflow, professional demonstration, AI-training task, or Project Odin-style exercise.
---

# Practising Technical Narration

## Overview

Help the user demonstrate expert reasoning during technical work. Capture investigation, decisions, trade-offs, implementation and verification - not commentary about clicks or typing.

## Safety First

Before recording, require a safe environment:

- Use a synthetic or supplied repository and data.
- Do not expose employer or client code, credentials or production data.
- Disable notifications and close unrelated applications.
- Check screen-recording and microphone permissions with a short test.

If the proposed material may be confidential, pause the exercise and replace it with a synthetic equivalent.

## Choose the Mode

### Generate a practice exercise

Tailor the task to experience the user has stated. Provide:

1. A self-contained brief without the solution.
2. A realistic timebox and success criteria.
3. Any repository assumptions or permitted tools.
4. Two or three edge cases the finished work should address.

Prefer debugging, implementation, refactoring or code-review work that can be completed in 20-45 minutes. Reveal hints or a reference solution only when requested.

### Coach the workflow

Use this sequence and scale its timing to the exercise:

1. **Frame:** Restate the problem, assumptions, constraints and success criteria.
2. **Investigate:** Reproduce the issue and follow evidence through the code, tests, logs or data.
3. **Hypothesise:** State a testable explanation and explain how it will be checked.
4. **Decide:** Compare plausible options and choose one using explicit trade-offs.
5. **Implement:** Make small, reviewable changes while explaining consequential decisions.
6. **Verify:** Test the happy path, relevant edge cases, regression risk and static checks.
7. **Summarise:** State the cause, change, evidence, remaining risks and production follow-up.

Encourage narration of intent, evidence, uncertainty, corrections, risks and validation. Quiet concentration is acceptable; do not narrate every keystroke.

### Review a transcript or recording notes

Score each category from 1 (missing) to 5 (excellent), cite specific evidence, then give the three highest-value improvements:

| Category | What good looks like |
|---|---|
| Safety | No confidential information, secrets or distracting notifications |
| Problem framing | Clear goal, assumptions, constraints and success criteria |
| Investigation | Evidence gathered before editing; hypotheses tested |
| Decisions | Alternatives and trade-offs explained without over-narrating |
| Execution | Small, coherent changes with risks considered |
| Verification | Meaningful tests, edge cases, final review and concise summary |

Do not infer competence from polish alone. Reward transparent corrections when evidence disproves an initial theory.

## Example Invocation

> Use $practising-technical-narration to give me a 40-minute TypeScript debugging exercise, coach my narration structure, and score my written transcript afterward.

## Common Mistakes

- Starting to edit before reproducing or understanding the problem.
- Speaking continuously instead of explaining decisions.
- Hiding uncertainty or silently changing direction.
- Completing the happy path without edge cases or regression checks.
- Ending without stating what changed and how it was verified.
