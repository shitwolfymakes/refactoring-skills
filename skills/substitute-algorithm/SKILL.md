---
name: substitute-algorithm
description: "Replace a function's body with a clearer algorithm that gives the same results. Use when you find a simpler approach or a library now does the job."
---

# Substitute Algorithm

*Source: Fowler, "Refactoring" (1st ed.), Ch. 6 — Composing Methods.*

**Problem:** There is a clearer way to compute what a function computes.
**Solution:** Replace the body of the function with the new algorithm.

## When to apply

- You've learned more about the problem and see a simpler approach.
- A library or the standard library now offers what your code does by hand.
- You need to change behavior slightly, and swapping to an algorithm that's easier to change comes first.

Decompose the function as far as possible first (`extract-method`). Substituting a large, complex algorithm in one go is very hard; small pieces make it tractable.

## Mechanics

1. Write the alternative algorithm and get it to build.
2. Run it against the tests. If the results match, you're done.
3. If they differ, use the old algorithm as an oracle: run each test case through both and compare outputs to find which cases diverge and why.

## Sketch

```
function foundPerson(people):
    for p in people:
        if p == "Don":  return "Don"
        if p == "John": return "John"
        if p == "Kent": return "Kent"
    return ""
```
becomes
```
function foundPerson(people):
    candidates = {"Don", "John", "Kent"}
    for p in people:
        if p in candidates: return p
    return ""
```

## Tips

- If test coverage is thin, add characterization tests that capture current outputs (including edge cases) before substituting.
- For complex algorithms, a temporary side-by-side comparison (run both, assert equal) in tests is a cheap safety net.

## Related

- Prepare with: `extract-method`.
