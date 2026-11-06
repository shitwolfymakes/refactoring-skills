---
name: push-down-method
description: "Move superclass behavior down to the only subclasses that need it. Use when a method applies to just some subtypes."
---

# Push Down Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** A superclass method only makes sense for some of its subclasses.
**Solution:** Move it to those subclasses.

## Mechanics

1. Declare the method in every subclass and copy the body into each.
   - It may need access to superclass fields: make them protected if you'll push the field down later, otherwise use (possibly protected) superclass accessors.
2. Remove the method from the superclass.
   - Callers may need to declare variables/parameters of the subclass type.
   - If it makes sense to call it through a superclass reference, the superclass is abstract, and you won't remove it from any subclass, declare it abstract on the superclass instead of removing it.
3. Build and test.
4. Delete it from the subclasses that have no use for it.
5. Build and test.

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Inverse: `pull-up-method`.
- Often with: `push-down-field`, `extract-subclass`, `replace-type-code-with-subclasses`.
- Signals Refused Bequest when many inherited features go unused.
