---
name: consolidate-duplicate-conditional-fragments
description: "Hoist code repeated in every branch of a conditional (or try/catch) to before or after it. Use when all branches share statements that hide what actually varies."
---

# Consolidate Duplicate Conditional Fragments

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions. Later editions fold it into "Slide Statements".*

**Problem:** Every branch of a conditional repeats the same piece of code.
**Solution:** Move it outside the conditional.

## Why

It makes clear what varies between branches and what stays the same.

## Mechanics

1. Identify code executed identically regardless of the condition.
2. Common code at the **start** of every branch → move it before the conditional.
3. Common code at the **end** of every branch → move it after the conditional.
4. Common code in the **middle**: check whether the code before/after it in each branch interacts with it. If not, slide it to the start or end, then move it out.
5. If more than one statement is moved, consider `extract-method` on it.
6. Build and test.

Same idea applies to `try`/`catch`: code duplicated at the end of the try block and every catch block belongs in `finally` (or after the construct).

## Sketch

```
if isSpecialDeal():
    total = price * 0.95
    send()
else:
    total = price * 0.98
    send()
```
becomes
```
if isSpecialDeal(): total = price * 0.95
else:               total = price * 0.98
send()
```

## Related

- `extract-method`, `decompose-conditional`.
