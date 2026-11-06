---
name: remove-setting-method
description: "Remove the setter of a field that should be fixed at construction and make it immutable. Use when a value must never change after creation."
---

# Remove Setting Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A field is meant to be fixed when the object is built and then left alone.
**Solution:** Remove any setting method for it.

## Why

A setter says "this field may change." If it shouldn't, don't provide one and make the field immutable — the intent is clear and the possibility of change is removed. This often happens when people use accessors everywhere by habit, including setters in constructors; consistency isn't worth the later confusion.

## Mechanics

1. Make the field immutable (`final`, `readonly`, `val`, `const`, etc.) if it isn't already. Build and test.
2. Check that the setter is called only from the constructor (or methods called by it).
3. Change the constructor to assign the field directly.
   - If a subclass sets a private superclass field via the setter: provide a protected superclass constructor (preferably) or initialization method. Don't give it a name that looks like a setter.
4. Build and test.
5. Remove the setter. Build.

## Language notes

- In languages with frameworks that need setters (some ORMs, serializers, dependency injection via setters), prefer constructor injection/creator annotations, or restrict setter visibility.
- In Python/JS without true immutability: drop the setter / property setter, use frozen dataclasses, `Object.freeze`, or `readonly` in TypeScript.
- Builders are a good way to keep immutability when construction is complex.

## Related

- Enables: `change-reference-to-value`.
- See also `encapsulate-field`, `hide-method`.
