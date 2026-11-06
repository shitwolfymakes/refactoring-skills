---
name: pull-up-method
description: "Move methods that do the same thing in sibling subclasses into the superclass. Use for duplicated subclass methods or overrides that match the parent."
---

# Pull Up Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Subclasses have methods with identical results.
**Solution:** Move them to the superclass.

## Why

Duplicate methods work fine today but breed bugs: change one and forget the other. Easiest case: identical bodies (copy-paste). Look carefully at any differences — they often reveal behavior you forgot to test.

Often comes after other steps: parameterize two methods separately until they become the same, then pull up. Also applies when a subclass override does exactly what the superclass method does (just delete the override).

If methods are similar but not identical in structure, consider `form-template-method`.

## Mechanics

1. Inspect the methods to ensure they're identical. If they do the same thing differently, `substitute-algorithm` on one to make them identical.
2. If signatures differ, change them to the one you want on the superclass (`rename-method`, `add-parameter`…).
3. Create the method in the superclass; copy one body over and adjust. Build.
   - If it calls something that both subclasses have but the superclass lacks, add that as an abstract method on the superclass.
   - If it uses a subclass field, `pull-up-field`, or `self-encapsulate-field` and declare an abstract getter on the superclass.
4. Delete one subclass method. Build and test.
5. Remove the subclass copies one at a time, testing after each, until only the superclass version is left.
6. Check callers: can any now declare the superclass type instead of a subclass?

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Inverse: `push-down-method`.
- Prerequisites: `pull-up-field`, `substitute-algorithm`, `parameterize-method`.
- For similar-but-different: `form-template-method`. Part of `extract-superclass`.
