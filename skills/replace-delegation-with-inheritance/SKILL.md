---
name: replace-delegation-with-inheritance
description: "Make a class that forwards its entire interface to a delegate a subclass of it. Use only when all of the delegate's interface is used and the delegate isn't shared state."
---

# Replace Delegation with Inheritance

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** You use delegation and keep writing many simple forwarding methods for the delegate's entire interface.
**Solution:** Have the forwarding class inherit from the delegate instead.

## When not to

- You **don't use all** the delegate's methods. A subclass must honor the superclass's whole interface. Alternatives for tedious forwarding:
  - `remove-middle-man` — let clients call the delegate directly.
  - `extract-superclass` or `extract-interface` for the common part, and inherit/implement that.
- The delegate is **shared by several objects and mutable**. Inheritance can't share data; that responsibility can't move into inheritance. (If the delegate is immutable, copying is safe.)
- The language or the delegate's class doesn't allow subclassing (final/sealed classes, or no class inheritance).

## Mechanics

1. Declare the forwarding class as a subclass of the delegate.
2. Build. Fix clashes (same name, different return type/exceptions/visibility) with `rename-method`.
3. Set the delegate field to the object itself (`this`/`self`).
4. Remove the simple forwarding methods.
5. Build and test.
6. Replace all other uses of the delegate field with calls on the object itself.
7. Remove the delegate field.

## Language notes

- In languages without class inheritance, the equivalent is struct embedding (Go) or a `Deref`-style forwarding (Rust — generally discouraged for this purpose); in Kotlin, interface delegation with `by` gives most of the benefit while keeping composition.

## Related

- Inverse: `replace-inheritance-with-delegation`.
- Alternatives: `remove-middle-man`, `extract-superclass`, `extract-interface`.
