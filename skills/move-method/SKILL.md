---
name: move-method
description: "Move a function to the class or module whose data it uses most (Move Function). Use for Feature Envy or over-coupled classes."
---

# Move Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects. Later editions: "Move Function".*

**Problem:** A method depends on, or is relied on by, another class or module more than its own.
**Solution:** Recreate the method in the class it depends on most. Leave the original as a one-line forwarder, or delete it.

## When to apply

- **Feature Envy:** a method references another object more than its own.
- Classes are too highly coupled, or one class has too much behavior.
- A good moment is right after moving some fields (`move-field`).

Before deciding, look at the method's callers, the methods it calls, and any overrides in the hierarchy. If the decision is hard, it probably doesn't matter much — go with instinct; you can move it again later. Moving other methods first often makes the choice obvious.

## Mechanics

1. Examine every feature of the source class the method uses. Anything only this method uses should move with it. If several methods share features, consider moving the group together.
2. Look through subclasses, superclasses, and other implementers for declarations of the same method. If it's polymorphic, you can only move it if the polymorphism can be expressed on the target.
3. Declare the method in the target. Rename it if a different name fits better there.
4. Copy the body and adapt it to its new home:
   - If it needs the source object, pass the source in as a parameter (or the specific values it needs).
   - For error/exception handling, decide which class should logically own it; leave handlers behind if the source should.
5. Build.
6. Determine how the source reaches the target object: an existing field or accessor, a new accessor, or — failing that — a new field (possibly temporary).
7. Turn the source method into a delegation to the target.
8. Build and test.
9. Decide whether to keep the delegator (easier when there are many callers) or remove it.
10. If removing, point every caller to the target method (search/replace or IDE), then build and test.

## Language notes

- In module-based languages, "class" means module/namespace/package; the coupling analysis is the same.
- Free functions: moving means changing which module owns it and updating imports.
- Use IDE "Move" refactorings where available; they update references and imports.

## Sketch

```
class Account:
    function overdraftCharge():
        if type.isPremium(): ... uses type's data heavily ...
```
becomes
```
class AccountType:
    function overdraftCharge(daysOverdrawn): ...
class Account:
    function overdraftCharge(): return type.overdraftCharge(daysOverdrawn)
```

## Related

- Often preceded by: `move-field`, `extract-method`.
- Larger-scale: `extract-class`, `inline-class`.
