---
name: split-temporary-variable
description: "Give each distinct use of a reassigned local its own variable (Split Variable). Use when one variable holds unrelated values; loop counters and accumulators excepted."
---

# Split Temporary Variable

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods. Later editions call it "Split Variable".*

**Problem:** A temporary variable is assigned more than once, and it is neither a loop variable nor a collecting (accumulator) variable.
**Solution:** Make a separate, immutable variable for each assignment.

## When to apply

A variable that holds the result of a computation for later reference should be set once. Being set several times means it has several responsibilities — and one name for two meanings misleads the reader.

## When not to

- **Loop variables** (e.g. `i` in a counting loop).
- **Collecting/accumulator variables** — later assignments look like `x = x + expr`, string concatenation, writing to a stream, appending to a collection.

## Mechanics

1. Rename the variable at its declaration and first assignment to a name that reflects that first use.
2. Make the new variable immutable.
3. Update all references up to the second assignment.
4. Declare a (new) variable at the second assignment.
5. Build and test.
6. Repeat stage by stage: rename at each assignment, update references until the next assignment.

## Sketch

```
temp = 2 * (height + width)
print(temp)
temp = height * width
print(temp)
```
becomes
```
const perimeter = 2 * (height + width)
print(perimeter)
const area = height * width
print(area)
```

## Related

- Enables: `replace-temp-with-query`, `extract-method`.
- Parameter variant: `remove-assignments-to-parameters`.
