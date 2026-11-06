---
name: replace-type-code-with-state-strategy
description: "Move a behavior-driving type code into a state or strategy object the host delegates to. Use when the code changes over the object's life or the class can't be subclassed."
---

# Replace Type Code with State/Strategy

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** A type code affects behavior, but you can't use subclassing (the code changes at runtime, or the class is already subclassed).
**Solution:** Move the varying behavior into a separate state/strategy hierarchy that the host holds and delegates to.

## State or Strategy?

The mechanics are identical. Use "Strategy" if you're simplifying one algorithm via `replace-conditional-with-polymorphism`; use "State" if state-specific data will move too and the object is best seen as switching between states.

## Mechanics

1. Self-encapsulate the type code (`self-encapsulate-field`).
2. Create a new class named for the purpose of the type code (e.g. `EmployeeType`) — the state object.
3. Add a subclass of it per type code value (easier all at once).
4. Add an abstract query on the state class returning the type code; each subclass overrides it with its value.
5. Build.
6. Add a field in the host for the state object.
7. Make the host's type-code getter delegate to the state object.
8. Make the host's type-code setter assign an instance of the right state subclass (often via a factory on the state class).
9. Build and test.
10. Next: `replace-conditional-with-polymorphism` to move conditional behavior into the state classes.

## Language notes

- Without inheritance: a state "interface"/trait with implementations, or a record of functions, works the same way.
- With first-class functions, a strategy with a single operation can simply be a function value.
- Sum types (enum with data, sealed class) + pattern matching are an alternative for a closed set of states.
- Stateless state objects can be shared singletons.

## Related

- Simpler alternative when possible: `replace-type-code-with-subclasses`.
- Next: `replace-conditional-with-polymorphism`.
- Used inside: `tease-apart-inheritance`, `extract-hierarchy`.
