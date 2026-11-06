---
name: encapsulate-collection
description: "Stop exposing a mutable collection: the getter returns a read-only view or copy, and add/remove methods replace the setter. Use when clients can change an object's internal collection behind its back."
---

# Encapsulate Collection

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** A method returns a collection (and maybe there's a setter for it).
**Solution:** Return a read-only view (or copy), and provide add/remove methods.

## Why

If the getter returns the actual collection, clients can change its contents without the owner knowing, and they learn too much about the internal data structure. A collection setter likewise bypasses the owner. Add/remove operations give the owner control (validation, back-pointers, invariants, events) and reduce coupling.

## Mechanics

1. Add `add<Item>` and `remove<Item>` methods.
2. Initialize the field to an empty collection.
3. Build.
4. Find callers of the setter. Either rewrite the setter in terms of add/remove, or change clients to call add/remove directly. Setters are typically used to fill an empty collection or replace a non-empty one; consider renaming (`rename-method`) to `initialize…`/`replace…`.
5. Build and test.
6. Wherever a caller mutates the collection it got from the getter, switch it to the add/remove methods. Build and test after each.
7. Make the getter hand out a copy or an unmodifiable view.
8. Build and test.
9. Look at the getter's users for logic that belongs on the owner (e.g. counting, filtering, summing) — `extract-method` + `move-method` it onto the owner.

## Language notes

Read-only options vary:
- Java: `Collections.unmodifiableList`, `List.copyOf`. C#: `IReadOnlyList<T>`, `AsReadOnly()`. Kotlin: expose `List` over a private `MutableList`.
- Python: return a `tuple`/`frozenset` or a copy; `types.MappingProxyType` for dicts.
- JS/TS: `ReadonlyArray<T>` type, `Object.freeze`, or return a copy (`[...items]`).
- Rust: return `&[T]` / an iterator; ownership rules do much of the work.
- Go: return a copy (`slices.Clone`) — slices share backing arrays.
- Returning an iterator/stream/sequence is often better than a collection.

A read-only *view* reflects later changes; a *copy* doesn't — choose deliberately.

## Related

- `encapsulate-field`, `move-method`.
