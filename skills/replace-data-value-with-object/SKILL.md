---
name: replace-data-value-with-object
description: "Turn a primitive that has grown behavior into its own type (Replace Primitive with Object). Use for Primitive Obsession, such as a phone number or money amount kept as a string."
---

# Replace Data Value with Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data. Later editions: "Replace Primitive with Object".*

**Problem:** A data item needs additional data or behavior.
**Solution:** Give the value its own type.

## When to apply

Early on, simple facts get simple representations (strings, numbers). Later they grow needs: formatting, parsing, validation, extracting parts (area code from a phone number), extra related data. When helper logic starts showing up in the owning class or across clients — the smells are duplication and feature envy — give the value its own type.

## Mechanics

1. Create a class for the value. Give it an immutable field of the original type, a getter, and a constructor taking that value.
2. Build.
3. Retype the field in the original class to use the new type.
4. Change the source's getter to return the new object's value (keeping the old interface for now).
5. Where the source constructor or setter assigns the field, construct the new object.
6. Build and test.
7. Then consider: should this be a shared reference (`change-value-to-reference`) or stay a value? Move related behavior onto the new type (`move-method`).

## Sketch

```
class Order: customer: String
```
becomes
```
class Customer: name: String
class Order:    customer: Customer
```

## Language notes

- Lightweight options: records/data classes (Kotlin, Java records, C# records, Python dataclasses), newtypes (Haskell, Rust tuple structs), named types (Go), branded types (TypeScript).
- Make it immutable and give it value equality unless you intend it as a reference object.

## Related

- Next steps: `change-value-to-reference`, `change-reference-to-value`.
- Siblings: `replace-type-code-with-class`, `replace-array-with-object`, `introduce-parameter-object`, `extract-class`.
