---
name: self-encapsulate-field
description: "Route a class's own field accesses through accessors. Use when subclasses need to override how a value is obtained, for lazy initialization, or before moving a field."
---

# Self Encapsulate Field

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** You access a field directly, but the coupling to the field is becoming awkward.
**Solution:** Create getter and setter methods for the field and use only those to access it — even inside the class.

## Direct vs indirect access

- **Indirect (via accessors):** subclasses can override how the value is obtained; enables lazy initialization, validation, change notification, and makes moving the field easier.
- **Direct:** easier to read — no need to wonder "is this just a getter?"

Reasonable default: use direct access (or follow your team's convention) until it gets in the way, then switch. Refactoring lets you change your mind.

**Most important case:** a superclass field whose access a subclass wants to override with a computed value. Self-encapsulating is the first step; then override the accessors.

## Mechanics

1. Create a getter and a setter for the field.
2. Replace every read with the getter and every write with the setter. Temporarily renaming the field makes the compiler/type checker find any you missed.
3. Make the field private.
4. Double-check all references are caught.
5. Build and test.

**Caution with constructors:** if the setter has behavior beyond assignment (validation, notification), it may not be safe to call during construction. Use direct assignment or a separate initialization method there.

## Language notes

- Languages with properties (C#, Kotlin, Swift, Python `@property`, JS getters/setters) let you switch from a plain field to computed access without changing call sites — the "accessor" may be a property.
- In languages without override (or for final classes), the main payoffs are lazy init and easier moving.

## Related

- Public variant: `encapsulate-field`.
- Prepares: `move-field`, `duplicate-observed-data`, `replace-type-code-with-state-strategy`, `change-bidirectional-association-to-unidirectional`.
