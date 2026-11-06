---
name: parameterize-method
description: "Merge functions that differ only in literal values into one that takes the value as a parameter. Use for near-duplicates like fivePercentRaise()/tenPercentRaise()."
---

# Parameterize Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions: "Parameterize Function".*

**Problem:** Several functions are alike except for literal values in their bodies.
**Solution:** Create one function that takes a parameter for the varying values.

## Why

Removes duplicate code and adds flexibility — other variations can be handled with other argument values.

## Mechanics

1. Create a parameterized function that can substitute for each repetitive one. (If only a fragment of each function is similar, `extract-method` that fragment first, then parameterize it.)
2. Build.
3. Turn one old function into a call to the new one (or replace its callers).
4. Build and test.
5. Repeat for each, testing after each.

## Sketch

```
function fivePercentRaise(): salary *= 1.05
function tenPercentRaise():  salary *= 1.10
```
becomes
```
function raise(factor): salary *= (1 + factor)
```

A subtler case: functions that compute bands (e.g. charging differently for ranges 0–100, 100–200, 200+) can become one function taking the band's bounds.

## When not to

- If the parameter becomes a flag that selects completely different code paths, you've gone too far — see `replace-parameter-with-explicit-methods`.

## Related

- Inverse: `replace-parameter-with-explicit-methods`.
- Uses: `extract-method`.
