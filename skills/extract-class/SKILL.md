---
name: extract-class
description: "Split a class doing two jobs by moving a cohesive set of fields and methods into a new class. Use for Large Class, Divergent Change, or Data Clumps."
---

# Extract Class

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A single class has taken on two jobs.
**Solution:** Move the fields and methods for one job into a new class.

## When to apply

Classes grow a responsibility at a time until they're too big to understand. Signs a split is possible:
- Some fields and some methods clearly belong together.
- Some data usually changes together, or is particularly interdependent.
- Ask: if I removed this field/method, which others would become meaningless? That group is a candidate class.
- Only a few features vary by subtype, or different groups of features would vary along different axes.

## Mechanics

1. Decide how to split the responsibilities.
2. Create a new class for the split-off responsibilities. If the old class's name no longer fits, rename it.
3. Link old → new (a field holding the new object). Add a back-link only when you find you need it.
4. `move-field` each field to move. Build and test after each.
5. `move-method` the methods, starting with low-level ones (called rather than calling) and working up. Build and test after each.
6. Review and reduce both classes' interfaces. If there's a two-way link, try to make it one-way.
7. Choose whether clients should see the new class at all. If so, choose: a reference object (shared, mutable) or an immutable value object.

## Sketch

```
class Person: name, officeAreaCode, officeNumber, getTelephoneNumber()
```
becomes
```
class Person:          name, officeTelephone, getTelephoneNumber()
class TelephoneNumber: areaCode, number, getTelephoneNumber()
```

## Language notes

- In non-OO code the same move applies to modules and record/struct types: split a large module or a large struct along the same seams.

## Related

- Inverse: `inline-class`.
- Steps: `move-field`, `move-method`.
- Alternatives: `extract-subclass` (when the split is about variation), `replace-data-value-with-object`, `introduce-parameter-object`.
- Afterwards: `change-reference-to-value` / `change-value-to-reference`, `hide-delegate`.
