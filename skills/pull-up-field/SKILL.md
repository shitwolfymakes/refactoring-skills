---
name: pull-up-field
description: "Move a field duplicated across sibling subclasses into their superclass. Use when subclasses hold the same data used the same way."
---

# Pull Up Field

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Two (or more) subclasses have the same field.
**Solution:** Move the field to the superclass.

## Why

Independently developed or recently merged subclasses often duplicate features. Duplicate fields may have different names; look at how they're used. If they're used the same way, generalize them. This removes the duplicated declaration and lets behavior that uses the field move up too (`pull-up-method`).

## Mechanics

1. Inspect all uses of the candidate fields to make sure they're used the same way.
2. If the names differ, rename them to the name you want on the superclass. Build and test.
3. Create the field in the superclass (with visibility subclasses can access, e.g. protected, or via accessors).
4. Delete the subclass fields.
5. Build and test.
6. Consider `self-encapsulate-field` on the new field.

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Inverse: `push-down-field`. Often followed by `pull-up-method`.
- Part of `extract-superclass`, `collapse-hierarchy`.
