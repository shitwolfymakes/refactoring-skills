---
name: extract-hierarchy
description: "Big refactoring: break a class full of flags and special-case conditionals into a hierarchy with one subclass per case. Use when a class keeps accreting variants."
---

# Extract Hierarchy

*Source: Fowler & Beck, "Refactoring" (1st ed.), Ch. 12 — Big Refactorings.*

**Problem:** A class does too much work, at least partly through many conditional statements.
**Solution:** Create a hierarchy of classes where each subclass represents a special case.

## Why

Evolutionary design: a class starts implementing one idea; then a flag and a couple of tests let it handle a new case; then another... A year later it's flags and conditionals everywhere. This strategy separates the strands.

It works when each object's variation is **fixed for its lifetime**. If the variation can change, first `extract-class` that aspect into its own object (then build the hierarchy there — state/strategy).

## Mechanics — variations unclear (one step at a time)

1. Identify one variation. (If it can change during an object's life, `extract-class` it out first.)
2. Add a subclass representing that one case. `replace-constructor-with-factory-method` on the original, and make the factory return the subclass where appropriate.
3. One at a time, copy methods containing conditional logic into the subclass, and simplify them using what you know is always true for that subclass. Use `extract-method` in the superclass to isolate conditional parts from unconditional parts if needed.
4. Repeat for other special cases until the superclass can be declared abstract.
5. Delete superclass method bodies overridden by every subclass; make them abstract.

## Mechanics — variations clear from the outset

1. Create a subclass for each variation.
2. `replace-constructor-with-factory-method` to return the right subclass. If variations are marked by a type code, `replace-type-code-with-subclasses` (or `replace-type-code-with-state-strategy` if they change at runtime).
3. Apply `replace-conditional-with-polymorphism` to methods with conditional logic; isolate varying parts with `extract-method` first when only part of a method varies.

## Language notes

- Without inheritance: one interface/trait with an implementation per variant, chosen by a factory; or a sum type with a variant per case if operations change more often than variants.

## Big refactorings — working style

Untangling a class like this is a long-term effort. Do the easy and obvious steps, then go back to productive feature work; return when you've learned more and do a few more. Keep the system working and tested throughout.

## Related

- Steps: `replace-constructor-with-factory-method`, `replace-conditional-with-polymorphism`, `extract-method`, `replace-type-code-with-subclasses`, `replace-type-code-with-state-strategy`, `extract-class`.
