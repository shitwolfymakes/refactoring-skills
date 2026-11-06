---
name: inline-class
description: "Fold a class that does too little into another and delete it. Use for Lazy Class, or to recombine two classes before splitting them a different way."
---

# Inline Class

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A class isn't doing very much.
**Solution:** Fold its features into another class and remove it.

## When to apply

- The class no longer pulls its weight — often because other refactorings moved its responsibilities away.
- Choose the absorbing class that uses the runt class the most.
- Also useful to merge two classes so you can re-split them along better lines with `extract-class`.

## Mechanics

1. Declare the public interface of the source class on the absorbing class; have those methods delegate to the source. (If a separate interface for those methods makes sense, `extract-interface` first.)
2. Point every reference to the old class at the class absorbing it. Tip: reduce the source's visibility and rename it, so the compiler/type checker/linter flags any dangling references.
3. Build and test.
4. `move-method` and `move-field` everything from source to absorbing class until nothing is left.
5. Delete the source class.

## Sketch

```
class Person:          name, officeTelephone, getTelephoneNumber()
class TelephoneNumber: areaCode, number, getTelephoneNumber()
```
becomes
```
class Person: name, areaCode, number, getTelephoneNumber()
```

## Related

- Inverse: `extract-class`.
- Smaller-scale equivalents: `inline-method`, `collapse-hierarchy`.
