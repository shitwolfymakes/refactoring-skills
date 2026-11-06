---
name: introduce-foreign-method
description: "Write a missing method for a class you can't change as a client-side helper taking the instance as its first argument. Use when workaround code for a library type repeats."
---

# Introduce Foreign Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A server class you use needs an additional method, but you can't modify it.
**Solution:** Write the method as a helper in your own code, taking the library object as its first argument.

## When to apply

- You code around a missing method more than once. One occurrence is fine; repetition should be factored into one place.
- Naming it as a foreign method signals that it really belongs on the server class.

If you need many foreign methods on the same class, or many client classes need the same one, use `introduce-local-extension` instead.

Foreign methods are a workaround. If you can, get the method onto its proper home — e.g. contribute it upstream or ask the owning team.

## Mechanics

1. Create a function in the client that does what you need. It should not use any of the client's own features — pass any values it needs as parameters.
2. Have it take the library object as its first parameter.
3. Mark it, e.g. with a comment "foreign method; should be in <ServerClass>", so you can find these later.

## Sketch

```
newStart = Date(previousEnd.year, previousEnd.month, previousEnd.day + 1)
```
becomes
```
newStart = nextDay(previousEnd)

// foreign method; should be on Date
private function nextDay(date): return Date(date.year, date.month, date.day + 1)
```

## Language notes

Many languages offer a first-class form of this — prefer it when idiomatic:
- **Extension methods/functions:** C#, Kotlin, Swift extensions, Dart extensions, Scala 3 extension methods/implicit classes.
- **Traits/typeclass impls on foreign types:** Rust traits (within coherence rules), Haskell typeclasses.
- **Free functions in a utility module:** Go, C, Python, JS/TS.
- Avoid monkey-patching shared library classes (Ruby refinements are a scoped alternative).

## Related

- For several methods: `introduce-local-extension`.
