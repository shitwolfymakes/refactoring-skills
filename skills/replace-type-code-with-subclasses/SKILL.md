---
name: replace-type-code-with-subclasses
description: "Replace a fixed type code that drives behavior with one subclass per value. Use when an object's kind is set at creation and switches on it are spreading."
---

# Replace Type Code with Subclasses

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** An immutable type code affects the behavior of a class.
**Solution:** Replace the type code with subclasses (one per code value).

## When to apply

- Conditionals (switch/case or if/else chains) test the type code and run different code. Those conditionals should become polymorphism (`replace-conditional-with-polymorphism`), which needs a hierarchy to host it. This refactoring builds that hierarchy.
- Some features are relevant only to certain type codes — afterwards use `push-down-method` / `push-down-field`.
- It moves knowledge of variants from clients into the class: adding a variant means adding a subclass, not hunting for every conditional. Most valuable when variants keep changing.

**Not applicable when:**
- The code changes after creation, or the class is already subclassed for another reason → `replace-type-code-with-state-strategy`.
- There are no conditionals on the code → `replace-type-code-with-class` is simpler.

## Mechanics

1. Self-encapsulate the type code (`self-encapsulate-field`). If the code is passed to the constructor, `replace-constructor-with-factory-method` so the factory can pick the subclass.
2. For each code value, create a subclass that overrides the type-code getter to return that value (hard-coded — temporary until conditionals are gone). Build and test after each.
3. Remove the type code field from the superclass; make the getter abstract.
4. Build and test.
5. Next: `replace-conditional-with-polymorphism` on the conditionals, then push down variant-specific features.

## Language notes

- Without class inheritance (Go, Rust, functional languages): model variants as types implementing a shared interface/trait, or as a sum type (tagged union/sealed class/enum with data) with exhaustive pattern matching. Exhaustive matching on a sealed type is a legitimate alternative to polymorphic dispatch, especially when operations change more often than variants.
- Prefer sealed/closed hierarchies (Kotlin/Java `sealed`, Scala, Swift enums) when the set of variants is fixed.

## Related

- Next step: `replace-conditional-with-polymorphism`, `push-down-method`, `push-down-field`.
- Alternatives: `replace-type-code-with-state-strategy`, `replace-type-code-with-class`.
- Prerequisites: `self-encapsulate-field`, `replace-constructor-with-factory-method`.
- Inverse-ish: `replace-subclass-with-fields`.
