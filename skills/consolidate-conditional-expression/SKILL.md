---
name: consolidate-conditional-expression
description: "Merge side-effect-free checks that lead to the same result into one named condition. Use when several ifs return the same value and really express one rule."
---

# Consolidate Conditional Expression

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions.*

**Problem:** A sequence of conditional tests all produce the same result.
**Solution:** Join them into one condition and pull it out into a named function.

## When to apply

- The separate checks are really one check (an OR of conditions, or nested ifs that are an AND). Consolidating makes that clear.
- It sets up `extract-method` on the condition, which replaces *what* you check with *why*.

**Don't** consolidate if the checks are genuinely independent and should be read as separate — the code already says that.

## Mechanics

1. Check that none of the conditions has side effects. If any do, stop (or first `separate-query-from-modifier`).
2. Replace the sequence with a single conditional using logical operators (sequential ifs → `or`; nested ifs → `and`).
3. Build and test.
4. Consider `extract-method` on the combined condition.

## Sketch

```
function disabilityAmount():
    if seniority < 2: return 0
    if monthsDisabled > 12: return 0
    if isPartTime: return 0
    ...
```
becomes
```
function disabilityAmount():
    if isNotEligibleForDisability(): return 0
    ...

function isNotEligibleForDisability():
    return seniority < 2 or monthsDisabled > 12 or isPartTime
```

## Related

- `decompose-conditional`, `extract-method`, `replace-nested-conditional-with-guard-clauses`.
