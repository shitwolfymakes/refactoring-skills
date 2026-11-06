---
name: inline-method
description: "Replace calls to a function with its body and delete it (Inline Function). Use when the body says as much as the name, or the indirection is pointless."
---

# Inline Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** A function's body says as much as its name does.
**Solution:** Put the body into each caller and remove the function.

## When to apply

- The body says exactly what the name says, so the indirection adds nothing.
- A chain of methods each just delegates to the next and you get lost in it — inline to flush out which indirections are useful.
- A group of functions is badly factored: inline them into one, then re-extract along better lines.
- Before `replace-method-with-method-object`: inline the helpers you want to bring along, so you move one method instead of several.

## When not to

- The function is polymorphic (overridden in subtypes / implemented by several implementations). Subtypes can't override what no longer exists.
- It's part of a published API used by code you can't change.
- Inlining is complicated by recursion, multiple return points, or access to private state of another object. If it gets complicated, don't do it.

## Mechanics

1. Check the function is not polymorphic (no overrides, no interface implementation relied upon).
2. Find all callers.
3. Replace each call with the body, adapting parameters to the arguments passed.
4. Build and test after each replacement (or after all, if trivial).
5. Remove the function definition.

## Sketch

```
function getRating():
    return moreThanFiveLateDeliveries() ? 2 : 1

function moreThanFiveLateDeliveries():
    return numberOfLateDeliveries > 5
```
becomes
```
function getRating():
    return numberOfLateDeliveries > 5 ? 2 : 1
```

## Related

- Inverse: `extract-method`.
- Similar, for variables: `inline-temp`. For classes: `inline-class`. For delegation layers: `remove-middle-man`.
