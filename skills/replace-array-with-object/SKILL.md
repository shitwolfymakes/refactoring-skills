---
name: replace-array-with-object
description: "Replace an array or tuple whose positions mean different things with an object with named fields. Use when code relies on conventions like row[0] being the name."
---

# Replace Array with Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** An array holds elements that mean different things.
**Solution:** Replace it with an object that has a field for each element.

## When to apply

Arrays (and lists, positional tuples) should hold a collection of *similar* things in some order. Conventions like "element 0 is the name" are hard to remember and fragile. Named fields convey the meaning, and once it's an object you can encapsulate it and move behavior onto it (`move-method`).

Same applies to: positional tuples returned from functions, untyped dicts/maps used as records with fixed keys, CSV rows passed around by index.

## Mechanics

1. Create a class to represent the information. Give it a (temporarily public) field holding the array.
2. Switch each user of the array over to the new class.
3. Build and test.
4. One element at a time, add accessors named for the element's purpose and change clients to use them. Build and test after each.
5. When all access goes through accessors, make the array private. Build.
6. For each element, create a real field and change its accessors to use it. Build and test after each.
7. When all elements are fields, delete the array.

## Sketch

```
row = new Array(3)
row[0] = "Liverpool"
row[1] = "15"
```
becomes
```
row = Performance()
row.name = "Liverpool"
row.wins = "15"
```

## Language notes

- Records/structs/data classes/named tuples/TypedDict/interfaces are the lightweight target. Choose correct field types (e.g. `wins` as an integer) in a follow-up step.

## Related

- Siblings: `replace-data-value-with-object`, `introduce-parameter-object`, `replace-record-with-data-class`.
