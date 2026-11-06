---
name: replace-parameter-with-explicit-methods
description: "Replace a function that branches on a flag or mode argument with one clearly named function per value (Remove Flag Argument). Use when the set of values is small and stable."
---

# Replace Parameter with Explicit Methods

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions: "Remove Flag Argument".*

**Problem:** A function switches its behavior on the value of an enumerated argument.
**Solution:** Create a separate function for each value of the parameter.

## Why

The caller already decides what it wants by choosing the argument — so give it a function to call directly. You remove the conditional, gain compile-time checking, and the interface is clearer: callers don't have to discover which parameter values are valid (usually poorly documented). Even without checking benefits, `switch.turnOn()` reads better than `switch.setState(true)`.

## When not to

- The parameter values change a lot, or are data rather than a mode (just setting a field) → use a simple setter.
- Behavior varies by type rather than by a caller's choice → `replace-conditional-with-polymorphism`.
- Callers compute the value dynamically and pass it through — explicit functions would just move the conditional to every caller.

## Mechanics

1. Create an explicit function for each parameter value.
2. Make each branch of the conditional call the corresponding new function. Build and test after each.
3. Change each caller to call the appropriate new function. Build and test.
4. When no callers remain, remove the conditional function.

## Sketch

```
function setValue(name, value):
    if name == "height": height = value; return
    if name == "width":  width = value;  return
    unreachable()
```
becomes
```
function setHeight(value): height = value
function setWidth(value):  width = value
```

## Related

- Inverse: `parameterize-method`.
- Alternative: `replace-conditional-with-polymorphism`.
