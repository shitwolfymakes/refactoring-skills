---
name: inline-temp
description: "Replace a variable assigned once from a simple expression with the expression (Inline Variable). Use when the variable adds nothing or blocks another refactoring."
---

# Inline Temp

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** A local assigned once from a simple expression is blocking other refactorings.
**Solution:** Replace every reference to the variable with the expression and remove the variable.

## When to apply

- Mostly as a step of `replace-temp-with-query`.
- On its own: a temp holds the result of a function call and blocks `extract-method` (or other refactorings). If it isn't in the way, it's usually harmless — leave it.

## When not to

- The expression has side effects, or its value can change between the assignment and the uses (evaluating it several times would change behavior).
- The expression is expensive *and* measured to matter. (Usually it doesn't.)
- The variable name is explaining something the expression doesn't — that's `introduce-explaining-variable`, the inverse.

## Mechanics

1. Check that the right-hand side is free of side effects.
2. Make the variable immutable (`final`, `const`, `val`, `let` without `mut`, etc.) and build — this proves it is assigned only once. In dynamic languages, search for assignments instead.
3. Replace each reference with the right-hand side. Build and test after each.
4. Remove the declaration and assignment.
5. Build and test.

## Sketch

```
total = cart.total()
return total > 50
```
becomes
```
return cart.total() > 50
```

## Related

- Inverse: `introduce-explaining-variable`.
- Usually a sub-step of `replace-temp-with-query`.
