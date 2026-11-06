---
name: introduce-null-object
description: "Replace null with an object that supplies default 'missing' behavior (Introduce Special Case). Use when many callers check for null and then do the same fallback."
---

# Introduce Null Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions. Later editions generalize it as "Introduce Special Case".*

**Problem:** Code keeps checking whether a value is null.
**Solution:** Substitute an object that stands in for the missing value.

## Why

Polymorphism means you call behavior without asking what kind of object you have. Null breaks that — every client must check first, and the fallback logic gets duplicated. A null object answers the same messages with sensible "missing" behavior (zero rate, empty collection, "occupant" as name, basic billing plan), so clients just call it.

Notes from practice:
- Null objects are great for display code, for absent optional services (a "missing" database session in development), and for absent collections (an empty bin with zero balance).
- They almost never blow up, which can make problems harder to notice — a null object in the wrong place behaves normally.
- Null objects are usually immutable, so a single shared instance is fine.

## Mechanics

1. Create a subclass (or implementation of the same interface) to act as the null version.
2. Add an `isNull()` (or `isMissing()`/`isUnknown()`) query: false on the real class, true on the null class. Optionally put it on a dedicated interface.
3. Build.
4. Find every place that can hand out null for this type; make it hand out the null object instead.
5. Find every comparison with null; replace with `isNull()`. Handle one place that produces nulls, and its callers, at a time, building and testing in between. Assertions that the value is never null in places where it shouldn't be any more can help.
6. Build and test.
7. Find clients that do "if not null, call X; otherwise do Y". Override X in the null class to do Y.
8. Remove those conditionals. Build and test after each.

## Sketch

```
plan = (customer == null) ? BillingPlan.basic() : customer.getPlan()
```
becomes
```
class NullCustomer extends Customer:
    isNull(): return true
    getPlan(): return BillingPlan.basic()

plan = customer.getPlan()
```

## Language notes

- In languages with **Option/Maybe** types (Rust, Haskell, Scala, Swift/Kotlin nullables), `map`/`unwrap_or`/`?:`/`??` often express the default more directly. Use a null object when the "missing" case has substantial behavior used in many places.
- In dynamic languages, the null object just needs the same methods (duck typing).
- Don't use a null object when callers genuinely need to handle absence differently each time — then explicit optionals are clearer.

## Related

- Specialization of `replace-conditional-with-polymorphism`.
- `introduce-assertion` to check no stray nulls remain.
