---
name: replace-conditional-with-polymorphism
description: "Move each branch of a type-based switch or if-chain into an override on a subtype or strategy. Use for the Switch Statements smell, especially when the same switch recurs."
---

# Replace Conditional with Polymorphism

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions.*

**Problem:** A conditional picks behavior based on what kind of object it has.
**Solution:** Move each branch to an overriding method in a subtype. Make the original method abstract.

## Why

With polymorphism you don't ask an object its type and then decide; you just call the behavior and each type does the right thing. The payoff is largest when the same type check is repeated across the code: adding a new type means adding one subtype rather than finding and updating every conditional. Clients don't need to know the subtypes, which reduces dependencies.

## Prerequisite: an inheritance structure

If you don't have one yet:
- Type fixed for the object's life and class not otherwise subclassed → `replace-type-code-with-subclasses` (simplest).
- Type changes at runtime, or class already subclassed → `replace-type-code-with-state-strategy`.
- Several switches on the same type code need only one hierarchy.

## Mechanics

1. If the conditional is part of a larger function, `extract-method` it out.
2. If needed, `move-method` so the conditional sits at the top of the hierarchy (on the superclass or state/strategy base).
3. Pick one subtype. Create an overriding method; copy that branch into it and adapt. (You may need to widen access to some superclass members.)
4. Build and test.
5. Remove that branch from the conditional. Build and test.
6. Repeat for each branch.
7. Make the superclass method abstract (or leave a sensible default if one branch is the common case).

## Sketch

```
function getSpeed():
    switch type:
        EUROPEAN:       return baseSpeed()
        AFRICAN:        return baseSpeed() - loadFactor() * numberOfCoconuts
        NORWEGIAN_BLUE: return isNailed ? 0 : baseSpeed(voltage)
```
becomes
```
abstract class Bird:  abstract getSpeed()
class European:      getSpeed(): return baseSpeed()
class African:       getSpeed(): return baseSpeed() - loadFactor() * numberOfCoconuts
class NorwegianBlue: getSpeed(): return isNailed ? 0 : baseSpeed(voltage)
```

## Trade-offs and language notes

- Polymorphism makes adding **types** easy; a sum type with exhaustive `match`/`switch` (Rust, Swift, Kotlin/Java sealed types, Haskell, TS discriminated unions) makes adding **operations** easy and lets the compiler find every place to update. In languages with exhaustiveness checking, a single well-placed match is not necessarily a smell; repeated matches on the same type across the codebase still are.
- Without inheritance: interfaces/traits with implementations, or a table/map from type to function.
- A one-off conditional on a stable set of cases is usually fine as is.

## Related

- Prerequisites: `replace-type-code-with-subclasses`, `replace-type-code-with-state-strategy`, `extract-method`, `move-method`.
- Special case for null checks: `introduce-null-object`.
- Inverse of a sort: `replace-parameter-with-explicit-methods` (dispatch by name instead of type).
