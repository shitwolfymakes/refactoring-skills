---
name: replace-parameter-with-method
description: "Drop a parameter the callee can compute itself (Replace Parameter with Query). Use to shorten parameter lists."
---

# Replace Parameter with Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions: "Replace Parameter with Query".*

**Problem:** A caller computes a value and passes it in, though the receiving function could compute it just as well.
**Solution:** Drop the parameter and have the receiver get the value itself.

## When to apply

A function shouldn't take as a parameter what it can find out on its own; long parameter lists are a burden on readers.

Possible when the calculation of the argument:
- doesn't depend on the calling function's parameters (those may differ per call), and
- the receiver can reach the needed object (it's the same object, or it holds a reference to the sender).

Remove parameters kept "for future parameterization" too — deal with it when needed; you may find it's the wrong parameter anyway. Exception: if changing the interface is very painful across the system — but then also look at reducing the dependencies that make it painful. Stable interfaces are good; freezing a poor one is not.

## When not to

- The receiver would need a new dependency on the sender that you don't want.
- Keeping the parameter keeps the function pure / referentially transparent, and that matters (e.g. testability, functional style). Passing values in can be the better design — see the inverse "Replace Query with Parameter" from later editions.

## Mechanics

1. If necessary, `extract-method` the calculation of the parameter.
2. Replace references to the parameter in the function body with calls to that function. Build and test after each.
3. `remove-parameter`.

## Sketch

```
subtotal = quantity * unitPrice
tier = customerTier()
finalPrice = priceForTier(subtotal, tier)
```
becomes
```
subtotal = quantity * unitPrice
finalPrice = priceForTier(subtotal)   // priceForTier calls customerTier() itself
```

## Related

- `remove-parameter`, `extract-method`, `preserve-whole-object`, `introduce-parameter-object`.
