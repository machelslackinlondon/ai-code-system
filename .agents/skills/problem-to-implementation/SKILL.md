---
name: problem-to-implementation
description: Use when solving coding exercises, practising technical interviews, or designing Node.js/TypeScript solutions whose assumptions, complexity, data structures, or production behaviour need validation.
---

# Problem to Implementation

Derive the solution from requirements, operations, and constraints rather than pattern-matching to a named algorithm.

## Choose the Interaction Mode

- **Interview/coaching:** Let the engineer propose each step. Use one targeted question or hint for unsupported choices. Reveal the solution only when requested or after they remain stuck.
- **Direct solution:** Present concise reasoning before implementation. Do not manufacture pauses when requirements are sufficient.
- **Real-world Node.js/TypeScript:** Use the same pipeline, then evaluate relevant operational behaviour.

Compress obvious stages; this is a framework, not a ceremonial checklist.

## Reasoning Contract

Establish these points before implementation:

| Stage | Required result |
|---|---|
| Problem | Inputs, outputs, rules, constraints, and material edge cases |
| Operations | Repeated operations the solution must support |
| Performance | Dominant operation and complexity justified by the constraints |
| Data structure | A structure whose operation costs satisfy that target |
| Algorithm | Logic operating on the structure, including its invariant |
| Correctness | Relevant normal, boundary, duplicate, empty, and impossible cases |
| Complexity | Time and space costs with their source, not just Big-O labels |
| Checkpoint | Confirmation that the approach is correct, simple enough, and constraint-safe |

Surface unknowns. Label only safe, revisable assumptions. Never weaken a requirement to preserve a preferred implementation; ask or choose explicitly when missing information changes architecture.

## Challenge Weak Reasoning

Identify the mismatch and consequence before redirecting:

- **Reasonable alternative:** compare trade-offs without forcing a change.
- **Correct but inefficient:** connect the repeated operation to the input bound, then ask how it could be cheaper.
- **Incorrect:** give a counterexample or explain the failing assumption, then ask a focused question.
- **Constraint or structure mismatch:** name the operation cost that breaks the target.

Example: before replacing DFS, ask: “What guarantees the first destination reached has the fewest edges? Which traversal order does?”

Urgency or sunk effort does not make an invalid approach implementable. If the user wants the solution, explain the correction and proceed.

## Translate Reasoning Into Code

After the checkpoint:

1. Write concise, language-independent pseudocode preserving the invariant.
2. Implement it directly with clear names and idiomatic JavaScript/TypeScript.
3. Test representative behaviour and relevant boundaries or failures; state what important tests prove.
4. Recheck implementation costs: repeated `Array.shift()` is linear, and numeric `sort()` needs a comparator.

## Production Review

Review relevant scale, memory, event-loop, concurrency, state, atomicity, reliability, observability, validation, and exhaustion risks. Add infrastructure only when justified; multi-instance consistency can require shared atomic state.

## Completed-Answer Shape

Use: problem → operations → performance → structure → algorithm/invariant → correctness → complexity → pseudocode → implementation → tests → production → alternatives.

In interview mode, reveal this progressively rather than emitting every section at once.
