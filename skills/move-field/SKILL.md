---
name: move-field
description: "Move a field to the class that uses it most and update its readers and writers. Use when another class depends on a field more than its owner does."
---

# Move Field

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** Another class uses a field (or soon will) more than the class that owns it.
**Solution:** Declare the field on the target class and update everything that uses it.

## When to apply

- More methods on another class use the field (directly or via accessors) than on its own class. If those methods sit well where they are, move the field instead of them.
- During `extract-class`: move fields first, then methods.

A design that was right last week can be wrong this week. That's normal; the only problem is not fixing it.

## Mechanics

1. If the field is publicly accessible, `encapsulate-field` first. If many methods access it or you'll move them too, consider `self-encapsulate-field`.
2. Build and test.
3. Create the field in the target with accessors (as appropriate for the language).
4. Build the target.
5. Determine how the source reaches the target: existing field/accessor, a new accessor, or a new (maybe temporary) field.
6. Remove the field from the source.
7. Replace every reference: reads become calls to the target's getter, writes to its setter. If the field wasn't private, check subclasses and other modules too.
8. Build and test.

With self-encapsulation, the change is local: only the source's getter/setter need to redirect to the target.

## Language notes

- In languages with properties, "getter/setter" may be property access; the redirection is the same.
- For persisted data (DB columns, serialized formats), the move also needs a data migration — plan it separately from the code move.

## Related

- Often followed by: `move-method`.
- Part of: `extract-class`, `inline-class`.
- Prepare with: `encapsulate-field`, `self-encapsulate-field`.
