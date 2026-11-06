---
name: extract-subclass
description: "Move features used by only some instances of a class into a new subclass. Use when a flag or type code marks a variation fixed at creation; consider Extract Class instead."
---

# Extract Subclass

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** A class has features used only by some instances.
**Solution:** Move those features into a new subclass.

## Subclass or delegate?

The main alternative is `extract-class` (delegation). Extract Subclass is usually simpler, but:
- An object's class can't change after creation; a delegate can be swapped.
- Subclassing can represent only one dimension of variation; for several, use delegation for all but one.

If the variation is signaled by a type code, see `replace-type-code-with-subclasses` / `replace-type-code-with-state-strategy`.

## Mechanics

1. Add an empty subclass of the original class.
2. Give it constructors (simplest: same arguments, call the super constructor). To hide the subclass from clients, `replace-constructor-with-factory-method`.
3. Find calls to the superclass constructors; where the subclass is needed, call the new constructor. Adjust constructor signatures with `rename-method` / `remove-parameter` as appropriate. If the superclass should no longer be instantiated directly, make it abstract.
4. One by one, `push-down-method` and `push-down-field` to shift features down. Moving methods before data tends to be easier here, the reverse of `extract-class`. Callers may need their declared types changed; the compiler/type checker helps. Build and test after each push.
5. Find fields that encode what the hierarchy now expresses (a boolean or type code). `self-encapsulate-field` and replace the getter with polymorphic constant methods. Refactor users of it with `replace-conditional-with-polymorphism` (moving outside methods in with `move-method` first if needed).
6. Build and test.

## Sketch

```
class JobItem: unitPrice, quantity, employee, isLabor
```
becomes
```
class JobItem:   unitPrice, quantity; getUnitPrice()
class LaborItem extends JobItem: employee; getUnitPrice() = employee.rate
```

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Alternative: `extract-class`. Siblings: `extract-superclass`, `extract-interface`.
- Steps: `push-down-method`, `push-down-field`, `replace-conditional-with-polymorphism`.
