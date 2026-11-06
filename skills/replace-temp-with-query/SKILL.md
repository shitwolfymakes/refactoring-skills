---
name: replace-temp-with-query
description: "Move the expression behind a local variable into a function and call it instead. Use when temps make a function long, block Extract Method, or are needed elsewhere."
---

# Replace Temp with Query

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** A local variable holds the result of an expression.
**Solution:** Extract the expression into a function (a "query"), replace all references to the variable with calls, and remove the variable. The query can now be used elsewhere too.

## When to apply

- Temps are visible only within one function, so they encourage long functions. A query is available to the whole class/module.
- As a preparatory step for `extract-method`: local variables are the main obstacle to extraction, so replace as many as you can.

## When not to (yet)

- The temp is assigned more than once → first `split-temporary-variable`.
- The expression has side effects → first `separate-query-from-modifier`.
- The computation is genuinely expensive and measured to matter — but don't assume; it usually doesn't, and it's easy to reintroduce a temp later. Better-factored code often reveals bigger optimizations.

## Mechanics

1. Find a temp assigned exactly once. (If more, `split-temporary-variable` first.)
2. Make it immutable and build to confirm single assignment.
3. Extract the right-hand side into a function. Make it private/module-local initially.
   - Ensure the new function has no side effects; if it does, `separate-query-from-modifier`.
4. Build and test.
5. Apply `inline-temp` to the variable.

### Loop accumulators

If a temp sums/collects values in a loop, extract the whole loop into the query. If one loop accumulates several totals, duplicate the loop — one per query. Simple loops are cheap to duplicate; clarity wins until profiling says otherwise.

## Sketch

```
subtotal = quantity * unitPrice
if subtotal > 500: return subtotal * 0.9
else:              return subtotal
```
becomes
```
if subtotal() > 500: return subtotal() * 0.9
else:                return subtotal()

function subtotal(): return quantity * unitPrice
```

## Language notes

- In functional or expression-oriented code, the "query" may be a pure function taking explicit arguments instead of a method on an object.
- Computed properties / getters are a natural fit where the language has them.

## Related

- Uses: `extract-method`, `inline-temp`.
- Prerequisites: `split-temporary-variable`, `separate-query-from-modifier`.
- Alternative when you can't: `introduce-explaining-variable`.
