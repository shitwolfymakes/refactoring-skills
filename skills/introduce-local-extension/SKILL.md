---
name: introduce-local-extension
description: "Add several missing methods to an unmodifiable class via your own subclass or wrapper. Use when helper functions for one external type are multiplying."
---

# Introduce Local Extension

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A server class you use needs several additional methods, but you can't modify it.
**Solution:** Collect the extra methods in a new class, either subclassing or wrapping the original. This is a *local extension*. Use it in place of the original.

## When to apply

- You've gone beyond one or two `introduce-foreign-method` helpers for the same type and they're getting out of hand.
- Keeping data and behavior packaged together makes them easier to find and reuse.

## Subclass vs wrapper

- **Subclass:** less work; supports everything the original does. But it must be applied at object creation. If you only get the object later, you must copy it — fine if the original is immutable, a bug if it's mutable (two objects with diverging state). The class must also allow subclassing.
- **Wrapper (decorator):** holds the original and forwards to it. Changes through the extension affect the original and vice versa. More forwarding code, and equality/identity need care (a wrapper and its original should usually compare equal on value).

## Mechanics

1. Create the extension as a subclass or wrapper of the original.
2. Add converting constructors that take the original as an argument (subclass: call the appropriate super constructor; wrapper: store it as the delegate).
3. Add the new features to the extension.
4. Use the extension in place of the original wherever the extra methods are needed.
5. Move any existing foreign methods for this class onto the extension.

## Language notes

Often the idiomatic modern form is not a subclass or wrapper at all:
- **Extension methods/functions** (C#, Kotlin, Swift, Dart, Scala 3) grouped in one file.
- **Traits implemented for the external type** (Rust, Scala), **typeclass instances** (Haskell).
- **Newtype wrappers** (Rust, Haskell, Go named types with methods) when you want a distinct type.
- In Go, embedding a struct gives wrapper-with-forwarding for free.

Choose based on whether you need a distinct type (wrapper/newtype) or just extra operations (extensions).

## Related

- For one or two methods: `introduce-foreign-method`.
- Wrapper mechanics resemble `replace-inheritance-with-delegation`.
