---
name: decompose-conditional
description: "Pull a complicated condition and each of its branches out into well-named functions. Use when if/else logic makes a function long or hides why it branches."
---

# Decompose Conditional

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions.*

**Problem:** A complicated conditional (if/then/else).
**Solution:** Pull the condition and each branch out into their own functions.

## When to apply

Complex conditionals are one of the most common sources of complexity. Both the condition and the branches tell you *what* happens but hide *why*. Naming the condition highlights what you're branching on; naming each branch highlights what each case means.

For nested conditionals, first consider `replace-nested-conditional-with-guard-clauses`; if that doesn't fit, decompose each conditional.

## Mechanics

1. `extract-method` on the condition.
2. `extract-method` on the then part and on the else part.
3. Build and test.

## Sketch

```
if date < SUMMER_START or date > SUMMER_END:
    charge = quantity * winterRate + winterServiceCharge
else:
    charge = quantity * summerRate
```
becomes
```
if notSummer(date): charge = winterCharge(quantity)
else:               charge = summerCharge(quantity)
```

## Language notes

- In expression-oriented languages the result is often a single conditional expression or match: `charge = notSummer(date) ? winterCharge(q) : summerCharge(q)`.

## Related

- Uses: `extract-method`. Alternatives/companions: `consolidate-conditional-expression`, `replace-nested-conditional-with-guard-clauses`, `replace-conditional-with-polymorphism`, `introduce-explaining-variable`.
