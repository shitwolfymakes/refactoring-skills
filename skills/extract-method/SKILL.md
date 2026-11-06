---
name: extract-method
description: "Turn a code fragment into a well-named function (Extract Function). Use when a function is long, a block needs a comment to explain it, a fragment is duplicated, or to prepare for other refactorings."
---

# Extract Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** A fragment of code can be grouped together and understood as one idea.
**Solution:** Move the fragment into a new function whose name states its *intention*, and call it from the original location.

## When to apply

- A function is long, or a reader has to mentally "chunk" it to understand it.
- A block is preceded by a comment explaining *what* it does — the comment is a candidate name.
- The same fragment is duplicated in several places.
- You want higher-level code to read like a sequence of comments.
- Fine-grained functions are easier to reuse and to override/substitute.

Length is not the criterion — the gap between *what the code does* and *how it does it* is. Extract even a single line if a name explains it better. If you can't think of a name more meaningful than the code itself, don't extract.

## Mechanics

1. Create a new function named after **what** it does, not **how**.
2. Copy the fragment into the new function.
3. Find variables the fragment references that are local to the source function (locals and parameters).
4. Locals that are used **only** inside the fragment: declare them inside the new function.
5. Locals **modified** by the fragment:
   - If exactly one is modified, make the new function a query that returns the new value, and assign the result at the call site.
   - If several are modified, or it's awkward: first try `split-temporary-variable` or `replace-temp-with-query`, then retry. Otherwise consider `replace-method-with-method-object`.
6. Locals only **read** by the fragment: pass them in as parameters.
7. Build / type-check.
8. Replace the fragment in the source with a call to the new function. Remove declarations in the source that are now unused.
9. Run the tests.

## Language notes

- In languages with multiple return values or tuples, returning several modified values is acceptable — but prefer one, and consider whether the values form a concept (`introduce-parameter-object`).
- Closures/nested functions can avoid parameter passing, but a top-level or member function is usually more reusable and testable.
- Most IDEs and language servers have an automated "Extract function/method" — prefer it when available, then review the name.

## Sketch

```
function printOwing(amount):
    printBanner()
    // print details
    print("name: " + name)
    print("amount: " + amount)
```
becomes
```
function printOwing(amount):
    printBanner()
    printDetails(amount)

function printDetails(amount):
    print("name: " + name)
    print("amount: " + amount)
```

## Related

- Inverse: `inline-method`.
- Enablers: `replace-temp-with-query`, `split-temporary-variable`, `remove-assignments-to-parameters`.
- Fallback when locals are too tangled: `replace-method-with-method-object`.
- Often followed by `move-method` if the extracted code envies another object.
