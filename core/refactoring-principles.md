# Refactoring Principles

## 1. Core Rule (Hard Constraint)

- Refactoring must preserve behaviour unless explicitly requested otherwise

---

## 2. Code Smell Detection (Signals Only)

Identify potential refactoring candidates when any of the following exist:

- Long methods (multiple responsibilities)
- Large classes (multiple unrelated concerns)
- Duplicate logic across modules or services
- Deep nesting or complex branching logic
- Excessive function parameters
- Tight coupling between unrelated components
- Feature envy (logic overly dependent on external modules)
- Poor testability due to hidden dependencies or structure

> Note: Code smells are indicators only and do not imply automatic refactoring.

---

## 3. Refactoring Decision Rules (WHEN to act)

Only propose refactoring when one or more of the following is true:

- Complexity is increasing maintenance cost
- Duplication is causing multi-location change risk
- Coupling is creating cascade-change risk
- Structure is actively blocking safe or readable implementation
- Testability or correctness is materially impacted

Do NOT refactor for:

- hypothetical future scalability
- stylistic preferences
- low-impact local improvements during feature work

---

## 4. Refactoring Constraints (HOW to behave)

- Never refactor during feature work unless required for correctness or safety
- Prefer localised refactors over system-wide changes
- Avoid structural changes unless impact is understood and scoped
- Ensure refactors are incremental and reversible

---

## 5. Refactoring Principle Summary

- Detect → Evaluate → Decide → Act
- Not all smells require refactoring
- Not all refactors should be applied immediately
