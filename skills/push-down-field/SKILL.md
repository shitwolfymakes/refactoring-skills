---
name: push-down-field
description: "Move a superclass field down to the only subclasses that use it. Use when superclass data matters only to some subtypes."
---

# Push Down Field

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** A field is used only by some subclasses.
**Solution:** Move the field to those subclasses.

## Mechanics

1. Declare the field in all subclasses.
2. Remove it from the superclass.
3. Build and test.
4. Remove it from all subclasses that don't need it.
5. Build and test.

If superclass code still references the field, push that code down first (`push-down-method`) or replace the reference with an abstract accessor implemented by subclasses.

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Inverse: `pull-up-field`.
- Often with: `push-down-method`, `extract-subclass`.
