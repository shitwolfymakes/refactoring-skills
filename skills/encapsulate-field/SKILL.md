---
name: encapsulate-field
description: "Make a public field private and give clients accessors (Encapsulate Variable). Use when outside code reads or writes an object's data directly."
---

# Encapsulate Field

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data. Later editions generalize it as "Encapsulate Variable".*

**Problem:** There's a public field.
**Solution:** Make it private and provide accessors.

## Why

Public data lets other objects read and change values without the owner knowing, separating data from the behavior that uses it. Keeping data and behavior together means changes happen in one place.

Encapsulating is only the first step. A class with nothing but accessors is a dumb data holder. Afterwards, look at the clients using the new accessors and see whether that logic should move into the class (`move-method`).

## Mechanics

1. Create a getter and setter for the field.
2. Locate every outside use of the field:
   - Reads → getter.
   - Assignments → setter.
   - Calling a mutating method on the field's object counts as a *read* (use the getter); only assignment uses the setter.
3. Build and test after each change.
4. When all clients are changed, make the field private.
5. Build and test.

Then: drop the setter if nothing needs it (`remove-setting-method`), and look for behavior to move in.

## Language notes

- With properties (C#, Kotlin, Swift, Python `@property`, JS/TS accessors), you can keep the same syntax at call sites while adding control later; still, decide deliberately what should be writable.
- In languages without access control (Python, JS without `#private`), use naming conventions (`_name`) and module exports to signal privacy.
- Same idea for module-level/global variables: wrap them in functions so all access is controlled.
- If the field is a collection, also apply `encapsulate-collection`.

## Related

- Internal variant: `self-encapsulate-field`.
- Follow-ups: `move-method`, `remove-setting-method`, `encapsulate-collection`.
