---
name: replace-method-with-method-object
description: "Turn a long function into an object whose fields hold its locals so it can be decomposed (Replace Function with Command). Use when tangled locals block Extract Method."
---

# Replace Method with Method Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods. Later editions call it "Replace Function with Command".*

**Problem:** A long function uses local variables in a way that prevents `extract-method`.
**Solution:** Turn the function into its own object, so all its locals become fields. Then decompose it into other methods on that object without passing parameters around.

## When to apply

- You've tried `replace-temp-with-query` and `split-temporary-variable`, and the locals are still too intertwined to extract pieces.
- Tip: first `inline-method` any helpers you want to bring along — moving one method is easier than moving several.

## Mechanics

1. Create a new class (or a closure/struct with functions) named after the function, e.g. `PriceCalculator` for `price()`.
2. Give it an immutable field for the source object that hosted the function, plus a field for each parameter and each local variable.
3. Give it a constructor taking the source object and each parameter.
4. Give it a single method, e.g. `compute()` / `execute()` / `run()`.
5. Copy the function body into `compute`. Route any calls on the original object through the source field.
6. Build.
7. Replace the original function's body with: construct the new object and call `compute`.
8. Now apply `extract-method` freely inside the new class — locals are fields, so no parameter passing is needed.

## Sketch

```
class Order:
    function price():
        primaryBasePrice = ...
        secondaryBasePrice = ...
        tertiaryBasePrice = ...
        // long computation
```
becomes
```
class Order:
    function price(): return PriceCalculator(this).compute()

class PriceCalculator:
    order; primaryBasePrice; secondaryBasePrice; tertiaryBasePrice
    constructor(order): this.order = order
    function compute(): ...   // now decompose into small methods
```

## Language notes

- Without classes: a module-level struct/record plus functions, or a closure capturing mutable state, achieves the same effect.
- Keep the method object private to the module unless it proves generally useful.

## Related

- Prefer simpler tools first: `replace-temp-with-query`, `extract-method`.
- Prepare with: `inline-method`.
