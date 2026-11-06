---
name: refactoring
description: "Refactoring workflow and principles (Fowler, Refactoring) plus an index of the refactoring skills. Use when asked to refactor, clean up, or restructure code without changing behavior, or to decide whether and how far to refactor."
---

# Refactoring — workflow and principles

*Source: Martin Fowler, "Refactoring: Improving the Design of Existing Code" (1st ed.). Principles from Ch. 1–2, 4–5 and 15; catalog from Ch. 6–12.*

**Refactoring (noun):** a change to how code is structured inside, made so it is easier to read and to change, **while what it does stays the same from the outside**.
**Refactoring (verb):** restructuring software by applying a series of such changes.

## Non-negotiables

1. **Behavior is preserved.** If behavior should change, that's a separate step (see "Two hats").
2. **Tests first.** Before refactoring, make sure there's a solid, self-checking, fast test suite for the code you'll touch. If there isn't, write characterization tests that pin down current behavior (including edge cases) before changing anything.
3. **Small steps.** Each step is one named refactoring (or one stage of its mechanics). After each step, build/type-check and run the tests. If a test fails, the bug is in the last small change — undo it or fix it immediately.
4. **Commit often** — after each green, meaningful step, so you can always roll back to a known-good state.

## Two hats

When developing you are either **adding function** or **refactoring** — never both at once. When adding function, don't restructure code; add capabilities and tests. When refactoring, don't add function; only restructure, and tests shouldn't need changing (except when an interface changes). Swap hats often, but know which one you're wearing.

## When to refactor

- **Rule of three:** first time, just do it; second time, wince at the duplication but do it anyway; third time, refactor.
- **When adding a feature:** first refactor so the feature is easy to add (Kent Beck: "make the change easy, then make the easy change"), and to understand the code.
- **When fixing a bug:** refactor to make the code clear enough that the bug becomes obvious.
- **During code review:** refactor to understand and to concretely try out suggestions.
- Use the `code-smells` skill to recognize *what* needs refactoring and pick refactorings.

## When not to

- The code doesn't work and needs rewriting from scratch — refactoring requires mostly working code.
- Very close to a hard deadline — the payoff comes later than the deadline. (But unfinished refactoring is technical debt: pay it down.)
- Code you won't touch again and don't need to understand.

## Practical guidance

- **Published vs public interfaces.** Changing an interface used by code you can't change (other teams, external users) needs care: keep the old interface, have it forward to the new one, mark it deprecated, and remove it later. Don't publish interfaces prematurely.
- **Databases and persisted/serialized data** are harder to refactor; separate the domain model from the storage schema, and plan data migrations explicitly.
- **Performance:** write well-factored code first, then measure with a profiler and optimize the hot spots only. Well-factored code is easier to tune. Don't avoid a refactoring for imagined performance costs — in most cases the difference is negligible.
- **Finding references:** use automated tools (IDE, language server, compiler errors) where possible. When searching by text, watch for polymorphism (overrides/implementations elsewhere in the hierarchy), reflection, dynamic dispatch, string-based lookups, serialization names, templates, and config files. Temporarily renaming a declaration and letting the compiler/type checker find uses is a reliable trick in static languages.
- **Have a goal, and stop when you lose confidence.** Refactor toward a specific design improvement. If you get lost or tests start failing in ways you don't understand, back up to the last green state and take smaller steps. Big refactorings (Ch. 12) are done a little at a time over weeks, alongside feature work.
- **Translating "compile" in the book's mechanics:** in dynamic languages, run the linter/type checker (e.g. mypy, tsc, eslint) and the tests at every "compile" step.

## How to use the per-refactoring skills

Each refactoring in the catalog has its own skill with *Problem → Solution*, when (not) to apply, step-by-step *Mechanics*, language notes, a sketch, and related refactorings. Follow the mechanics in order, testing between steps. Prefer automated IDE refactorings where available, then verify.

### Ch. 6 — Composing Methods
`extract-method`, `inline-method`, `inline-temp`, `replace-temp-with-query`, `introduce-explaining-variable`, `split-temporary-variable`, `remove-assignments-to-parameters`, `replace-method-with-method-object`, `substitute-algorithm`

### Ch. 7 — Moving Features Between Objects
`move-method`, `move-field`, `extract-class`, `inline-class`, `hide-delegate`, `remove-middle-man`, `introduce-foreign-method`, `introduce-local-extension`

### Ch. 8 — Organizing Data
`self-encapsulate-field`, `replace-data-value-with-object`, `change-value-to-reference`, `change-reference-to-value`, `replace-array-with-object`, `duplicate-observed-data`, `change-unidirectional-association-to-bidirectional`, `change-bidirectional-association-to-unidirectional`, `replace-magic-number-with-symbolic-constant`, `encapsulate-field`, `encapsulate-collection`, `replace-record-with-data-class`, `replace-type-code-with-class`, `replace-type-code-with-subclasses`, `replace-type-code-with-state-strategy`, `replace-subclass-with-fields`

### Ch. 9 — Simplifying Conditional Expressions
`decompose-conditional`, `consolidate-conditional-expression`, `consolidate-duplicate-conditional-fragments`, `remove-control-flag`, `replace-nested-conditional-with-guard-clauses`, `replace-conditional-with-polymorphism`, `introduce-null-object`, `introduce-assertion`

### Ch. 10 — Making Method Calls Simpler
`rename-method`, `add-parameter`, `remove-parameter`, `separate-query-from-modifier`, `parameterize-method`, `replace-parameter-with-explicit-methods`, `preserve-whole-object`, `replace-parameter-with-method`, `introduce-parameter-object`, `remove-setting-method`, `hide-method`, `replace-constructor-with-factory-method`, `encapsulate-downcast`, `replace-error-code-with-exception`, `replace-exception-with-test`

### Ch. 11 — Dealing with Generalization
`pull-up-field`, `pull-up-method`, `pull-up-constructor-body`, `push-down-method`, `push-down-field`, `extract-subclass`, `extract-superclass`, `extract-interface`, `collapse-hierarchy`, `form-template-method`, `replace-inheritance-with-delegation`, `replace-delegation-with-inheritance`

### Ch. 12 — Big Refactorings
`tease-apart-inheritance`, `convert-procedural-design-to-objects`, `separate-domain-from-presentation`, `extract-hierarchy`

## Reporting

When refactoring for a user, say which refactorings you applied (by name), confirm tests passed after each meaningful step (or say plainly if tests are missing or failing), and keep any behavior changes out of the refactoring commits — call them out separately.
