---
name: extract-superclass
description: "Give classes with similar features a common superclass and pull the shared parts up. Use when separate classes duplicate data and behavior; consider Extract Class instead."
---

# Extract Superclass

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Two classes have similar features.
**Solution:** Introduce a shared superclass and pull the common parts up into it.

## Why

Duplicate code means changing several places when you need to change one thing. Two classes doing similar things (the same way or differently) are a form of duplication. You often don't see the commonality until both classes exist, so the hierarchy is created after the fact.

**Inheritance or delegation?** The alternative is `extract-class`. Inheritance is the simpler choice when the classes have the same interface, not just similar code. You only get one superclass, though. If it turns out wrong, `replace-inheritance-with-delegation` later.

## Mechanics

1. Create an empty abstract superclass; make the original classes its subclasses.
2. Move shared elements up one at a time with `pull-up-field`, `pull-up-method`, and `pull-up-constructor-body`. Fields first is usually easier.
   - Same purpose, different signatures → `rename-method` to align, then pull up.
   - Same signature, different bodies → declare it abstract on the superclass.
   - Bodies that differ but compute the same result → try `substitute-algorithm` to make them identical, then pull up.
   - Build and test after each pull.
3. Examine what's left in the subclasses. Common parts → `extract-method` + `pull-up-method`. Similar overall flow → `form-template-method`.
4. Check clients of the subclasses: if they only use the common interface, change their declared type to the superclass.

## Sketch

```
class Employee:   name, id, annualCost
class Department: name, staff, totalAnnualCost(), headCount()
```
becomes
```
abstract class Party: name; abstract annualCost()
class Employee extends Party:   id, annualCost
class Department extends Party: staff; annualCost(); headCount()
```

## Language notes

- Without class inheritance (Go, Rust, many functional languages), the equivalent move is between concrete types and a shared abstraction: default methods on a trait/interface, an embedded struct (Go), or a shared helper/module the variants delegate to.
- Mixins/traits (Ruby modules, Scala/PHP traits, Python mixins) are also valid destinations for shared members.

## Related

- Alternatives: `extract-class`, `extract-interface`.
- Steps: `pull-up-field`, `pull-up-method`, `pull-up-constructor-body`, `form-template-method`.
- Inverse: `collapse-hierarchy`.
