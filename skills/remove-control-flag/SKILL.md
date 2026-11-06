---
name: remove-control-flag
description: "Replace a loop-control flag with break, continue, or return. Use when done/found flags obscure the logic."
---

# Remove Control Flag

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions. Later editions: "Replace Control Flag with Break".*

**Problem:** A variable acts as a control flag for a series of boolean expressions.
**Solution:** Use `break`, `continue`, or `return` instead.

## Why

```
done = false
while not done:
    if condition:
        doSomething()
        done = true
    ...
```
Control flags come from the "one entry, one exit" rule. One entry is enforced by modern languages; one exit leads to convoluted conditionals with awkward flags. Removing the flag makes the real purpose of the conditional much clearer.

## Mechanics — with break/continue

1. Identify the flag value that ends the loop or logic.
2. Replace assignments of that value with `break` or `continue`.
3. Build and test after each.
4. Remove the flag when unused.

## Mechanics — with return (preferred; works in any language)

1. `extract-method` on the logic containing the flag.
2. Find the flag value that gets you out.
3. Replace assignments of that value with `return`.
4. Build and test after each.

`return` clearly signals that nothing more in the function runs. Prefer it even when `break` is available.

**If the flag also carries a result** (e.g. `found` plus the found item): with `break` you still need a variable for the result; with an extracted function you just return the value.

## Language notes

- Functional/collection operations often remove the loop entirely: `any`, `find`, `first`, `takeWhile`, `some`, `exists`.
- Labeled break/continue (Java, Kotlin, Go, Rust, JS) exit nested loops — but extracting a function and returning is usually clearer.

## Related

- `extract-method`, `replace-nested-conditional-with-guard-clauses`, `substitute-algorithm`.
