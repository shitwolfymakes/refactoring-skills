---
name: encapsulate-downcast
description: "Return the specific type instead of making callers cast the result. Use when callers repeatedly downcast or type-assert a function's return value."
---

# Encapsulate Downcast

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A function returns an object that its callers need to downcast.
**Solution:** Move the downcast into the function, and declare the more specific return type.

## Why

Downcasting tells the compiler something it should know. If you know the returned value's type is more specific than the signature says, don't push that work (and risk) onto every caller — return the most specific type you can.

## Mechanics

1. Find places where callers must cast the result of a function. These often involve functions returning a collection, iterator, or a generic base type.
2. Move the cast into the function and change its declared return type.
3. For functions returning collections, also apply `encapsulate-collection` — and look at what callers use the collection for, and provide that operation directly.
4. Build and test.

## Sketch

```
function lastReading(): Object   return readings.last()
reading = (Reading) station.lastReading()
```
becomes
```
function lastReading(): Reading  return readings.last()
reading = station.lastReading()
```

## Language notes

- In languages with **generics** (Java 5+, C#, TS, Kotlin, Swift, Rust, Go 1.18+), the real fix is usually to make the collection or container generically typed so no cast is needed at all.
- In dynamic languages there are no casts, but the same idea applies to type checks (`isinstance`, `typeof`) callers repeat — return a well-defined type and document it with type hints.
- Go/TS type assertions and Rust `downcast_ref` are the same smell.

## Related

- `encapsulate-collection`.
