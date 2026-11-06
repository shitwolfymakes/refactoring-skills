---
name: collapse-hierarchy
description: "Merge a superclass and subclass that are no longer meaningfully different. Use when a subclass adds little, or a speculative hierarchy never grew variants."
---

# Collapse Hierarchy

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** A superclass and its subclass have become nearly the same.
**Solution:** Merge them together.

## Why

Hierarchies get tangled over time. After moving fields and methods up and down, a subclass may no longer add value. Also typical for Speculative Generality: an abstract base with a single implementation that was "for the future".

## Mechanics

1. Choose which class to remove: the superclass or the subclass. (Keep the one whose name fits the merged concept better.)
2. Use `pull-up-field` / `pull-up-method`, or `push-down-method` / `push-down-field`, to move all behavior and data from the removed class into the remaining one. Build and test after each move.
3. Change references to the removed class (variable/parameter types, constructors, factory calls, type checks) to use the merged class.
4. Remove the empty class.
5. Build and test.

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Inverse: `extract-superclass` / `extract-subclass`.
- Siblings: `inline-class`, `replace-subclass-with-fields`.
