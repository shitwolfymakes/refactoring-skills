---
name: replace-exception-with-test
description: "Check a condition up front instead of catching the exception it causes (Replace Exception with Precheck). Use when exceptions serve as ordinary control flow."
---

# Replace Exception with Test

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions: "Replace Exception with Precheck".*

**Problem:** An exception is raised (or caught) for a condition the caller could easily test beforehand.
**Solution:** Have the caller check the condition before calling.

## Why

Exceptions are for exceptional behavior — unexpected errors. They shouldn't substitute for conditional tests. If the caller can reasonably check the condition before the operation, provide a test and use it.

## Mechanics

1. Add a test up front; move the handler code from the catch into the matching branch of the new if.
2. Add an assertion (or logging) in the catch block, so you notice if it's still reached.
3. Build and test.
4. Delete the catch, and the whole try if nothing else is caught there.
5. Build and test.

## Sketch

```
function getValueForPeriod(n):
    try: return values[n]
    catch IndexOutOfBounds: return 0
```
becomes
```
function getValueForPeriod(n):
    if n >= values.length: return 0
    return values[n]
```

## Language notes

- Some ecosystems favor "easier to ask forgiveness than permission" (EAFP), notably Python — e.g. `try: d[key] except KeyError`. That's idiomatic there, and `dict.get(key, default)` is better than either. Use the language's idiom; the smell is *expensive or obscure* exception use for *routine* conditions.
- Check-then-act can race under concurrency (file exists → open; key present → read from a shared map). There, attempting the operation and handling the failure is correct.
- Prefer APIs that return optionals/defaults (`get`, `getOrDefault`, `TryParse`, `.get()` returning `Option`) over both.

## Related

- Inverse direction for real errors: `replace-error-code-with-exception`.
- `introduce-assertion`.
