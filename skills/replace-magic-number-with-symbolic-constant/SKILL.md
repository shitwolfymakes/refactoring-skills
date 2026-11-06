---
name: replace-magic-number-with-symbolic-constant
description: "Replace an unexplained literal with a named constant (Replace Magic Literal). Use when a literal's meaning isn't obvious, it repeats, or it may change."
---

# Replace Magic Number with Symbolic Constant

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data. Later editions: "Replace Magic Literal".*

**Problem:** A literal number (or string) has a particular meaning that isn't obvious.
**Solution:** Create a constant named after the meaning and replace the literal with it.

## When to apply

- The meaning of the literal isn't obvious from context.
- The same logical value appears in several places — changing it would be a hunt.
- Named constants are free at runtime in most languages and greatly improve readability.

**Look for a better alternative first:**
- The literal is a type code → `replace-type-code-with-class` (or an enum).
- It's a collection's length → use the collection's size/length.
- It's configuration that varies by environment → make it configuration, not a constant.

Don't name constants after their value (`ONE_HUNDRED = 100`) — name them after their meaning. Obvious literals (0, 1, `""`, in their obvious roles) don't need names.

## Mechanics

1. Declare a constant set to the literal's value.
2. Find all occurrences of the literal.
3. For each, check that it really means the same thing as the constant (the same number can mean different things). If so, replace it.
4. Build.
5. When all are replaced, build and test — behavior should be unchanged.
   - Good check: change the constant's value and see whether the program follows (you may need to adjust expected results). Not always possible, but useful when it is.

## Sketch

```
function potentialEnergy(mass, height): return mass * 9.81 * height
```
becomes
```
const STANDARD_GRAVITY = 9.81
function potentialEnergy(mass, height): return mass * STANDARD_GRAVITY * height
```

## Related

- `replace-type-code-with-class`.
