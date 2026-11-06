---
name: remove-assignments-to-parameters
description: "Use a local variable instead of reassigning a parameter. Use when a function rebinds its parameters, blurring what was passed in."
---

# Remove Assignments to Parameters

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** The code assigns to a parameter.
**Solution:** Use a local variable instead.

"Assigning to a parameter" means rebinding the name to a different value or object. Mutating the object the parameter refers to (calling methods on it) is a different thing and is not the target here.

## Why

- **Clarity:** the parameter should consistently mean "what the caller passed in".
- **Semantics confusion:** in pass-by-value languages (most mainstream languages, including those that pass object references by value), rebinding is invisible to the caller, which surprises people used to pass-by-reference. In pass-by-reference languages it *is* visible, which surprises everyone else.

In languages with explicit output/reference parameters (`ref`, `out`, `inout`, `&mut`, Pascal `var`, C++ references), assignment may be the intended contract — but prefer return values over output parameters where possible.

## Mechanics

1. Create a local variable initialized from the parameter.
2. Every use of the parameter **after** the assignment should read the local instead.
3. Make the assignment target the local.
4. Build and test.
5. If the language passes this parameter by reference: check whether callers use the value afterward. Try to return a single value instead. If several values flow back, consider turning them into an object (`introduce-parameter-object`) or splitting the function.

## Sketch

```
function discount(inputVal, quantity, yearToDate):
    if inputVal > 50: inputVal -= 2
    ...
```
becomes
```
function discount(inputVal, quantity, yearToDate):
    result = inputVal
    if inputVal > 50: result -= 2
    ...
```

## Language notes

- Many linters have a "no parameter reassignment" rule; some languages make parameters immutable by default.

## Related

- `split-temporary-variable` (same idea for locals).
- Enables `extract-method`.
