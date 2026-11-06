---
name: replace-nested-conditional-with-guard-clauses
description: "Handle special cases with early returns at the top so the normal path is unindented. Use when nested if/else hides the main flow."
---

# Replace Nested Conditional with Guard Clauses

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions.*

**Problem:** A function's conditional behavior doesn't make the normal path of execution clear.
**Solution:** Deal with each special case up front in a guard clause.

## Why

Conditionals come in two kinds:
- **Both branches are normal behavior** → use if/else; it gives the branches equal weight.
- **One branch is unusual** → check it and return/throw immediately: a *guard clause*. It says "this is rare; if it happens, deal with it and get out."

"One exit point" is not a useful rule. Clarity is: use one exit if it's clearer, otherwise don't.

## Mechanics

1. For each special-case check, turn it into a guard clause that returns (or throws).
2. Build and test after each.
3. If all guards yield the same result, `consolidate-conditional-expression`.

### Reversing conditions

Sometimes the nesting is around the *normal* case (`if capital > 0: if rate > 0 and duration > 0: …`). Invert each condition so the guard handles the exceptional case: `if capital <= 0: return result`. Take care with negations of compound conditions.

## Sketch

```
function getPayAmount():
    if isDead: result = deadAmount()
    else:
        if isSeparated: result = separatedAmount()
        else:
            if isRetired: result = retiredAmount()
            else: result = normalPayAmount()
    return result
```
becomes
```
function getPayAmount():
    if isDead:      return deadAmount()
    if isSeparated: return separatedAmount()
    if isRetired:   return retiredAmount()
    return normalPayAmount()
```

## Language notes

- Swift `guard … else { return }`, Rust `let … else`, early `?` error propagation, and Go's `if err != nil { return err }` style are all built around guard clauses.

## Related

- `consolidate-conditional-expression`, `decompose-conditional`, `remove-control-flag`, `replace-error-code-with-exception`.
