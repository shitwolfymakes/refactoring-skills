---
name: replace-record-with-data-class
description: "Wrap a raw record (legacy struct, API payload, DB row, untyped dict) in a simple class with named fields (Encapsulate Record). Use as a first step before adding behavior."
---

# Replace Record with Data Class

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data. Later editions: "Encapsulate Record".*

**Problem:** Your code has to work with record-shaped data from outside: a legacy program's records, an external API's payload, a database row.
**Solution:** Wrap it in a plain data class.

## When to apply

- Bringing record-shaped data into your program: legacy ports, external API payloads (JSON), database rows, message formats, untyped dictionaries passed around.
- Start by making the class mirror the external record exactly. Move behavior and reshape fields in later refactorings.
- If it's an array where each index means something, use `replace-array-with-object`.

## Mechanics

1. Add a class that stands for the record.
2. Give it a private field with a getter (and setter if needed) for each data item.
3. The result is a plain data holder with no behavior yet. Further refactoring (`move-method`, `encapsulate-collection`, `remove-setting-method`) gives it some.

## Language notes

- Typed records/data classes/structs are usually the right shape: Java/C# records, Kotlin data classes, Python dataclasses or pydantic models, TS interfaces/classes, Rust/Go structs.
- At a system boundary, parse and validate into the type once, rather than passing raw maps deeper into the code.

## Related

- `replace-array-with-object`, `replace-data-value-with-object`, `encapsulate-field`.
- A data class with only accessors is the Data Class smell — the next step is moving behavior to it.
