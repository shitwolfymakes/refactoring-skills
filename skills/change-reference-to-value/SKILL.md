---
name: change-reference-to-value
description: "Turn a small shared reference object into an immutable value object compared by value. Use when managing shared instances of something like a currency or date range is a burden."
---

# Change Reference to Value

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** Shared instances of a small, unchanging reference object are more trouble to manage than they are worth.
**Solution:** Turn it into a value object.

## When to apply

Reference objects must be controlled — you always ask a registry for the right instance, and the links can be awkward. Value objects can be freely copied and are especially useful in concurrent and distributed systems.

**Immutability is essential.** Querying a value object must always give the same result, so many copies representing the same thing are harmless. If it must be mutable and changes must propagate, keep it a reference.

Immutable doesn't mean "never changes in the domain": a salary changes by *replacing* the money object, not by mutating its amount.

## Mechanics

1. Check that the object is immutable or can become so. Use `remove-setting-method` until it is. If it can't become immutable, abandon this refactoring.
2. Implement value-based equality **and** a consistent hash.
3. Build and test.
4. Consider removing the factory/registry and exposing a constructor.

## Language notes

- Records/data classes/structs often give value equality for free (Kotlin data class, Java/C# records, Python frozen dataclasses, Rust `#[derive(PartialEq, Eq, Hash)]`, Scala case classes).
- In JS/TS there is no built-in value equality; provide an `equals` method or compare by a canonical key.
- Equality and hash must agree, or hash-based collections will misbehave.

## Related

- Inverse: `change-value-to-reference`.
- Prerequisite: `remove-setting-method`.
