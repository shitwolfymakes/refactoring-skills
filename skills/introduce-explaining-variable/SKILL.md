---
name: introduce-explaining-variable
description: "Name a complex expression or its parts with local variables (Extract Variable). Use when a condition or calculation is hard to read."
---

# Introduce Explaining Variable

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods. Later editions call it "Extract Variable".*

**Problem:** An expression is complicated and hard to read.
**Solution:** Store the expression, or pieces of it, in immutable locals whose names say what they mean.

## When to apply

- Compound conditionals: name each clause so the condition reads as intent.
- Long calculations: name each step.

Prefer `extract-method` when you can — a function is reusable across the class/module, a local is not. Use an explaining variable when local variables make extraction awkward, or the name only matters in this one place.

## Mechanics

1. Declare an immutable local and assign it part of the complex expression.
2. Replace that part of the expression with the variable. If the part is repeated, replace repeats one at a time.
3. Build and test.
4. Repeat for other parts.

## Sketch

```
if platform.upper().contains("MAC") and browser.upper().contains("IE")
   and wasInitialized() and resize > 0:
    ...
```
becomes
```
isMacOs     = platform.upper().contains("MAC")
isIEBrowser = browser.upper().contains("IE")
wasResized  = resize > 0
if isMacOs and isIEBrowser and wasInitialized() and wasResized:
    ...
```

## Related

- Inverse: `inline-temp`.
- Often a stepping stone to `extract-method` / `replace-temp-with-query`.
- For whole conditionals: `decompose-conditional`.
