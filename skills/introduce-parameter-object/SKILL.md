---
name: introduce-parameter-object
description: "Replace parameters that always travel together with one object, then move shared behavior onto it. Use for Long Parameter List and Data Clumps."
---

# Introduce Parameter Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A group of parameters naturally goes together.
**Solution:** Replace them with an object.

## Why

- A group passed together to several functions (on one or several classes) is a **data clump** — it deserves a type.
- Shorter parameter lists; consistent, named access to the values.
- The deeper benefit: once the data is grouped, behavior that manipulates it (range checks, conversions, comparisons) can move onto the new type, removing duplicated code across callers.

## Mechanics

1. Create a class for the group of parameters. Make it immutable.
2. Build.
3. `add-parameter` for the new object, passing null/placeholder at all callers. (With many callers, keep the old signature delegating to the new one and migrate callers gradually.)
4. For each parameter in the clump: remove it from the signature, change the body and callers to use the parameter object's value instead. Build and test after each.
5. Use `move-method` to shift related behavior onto the new object (use `extract-method` first if it's part of a function).

## Sketch

```
amountInvoicedIn(start, end)
amountReceivedIn(start, end)
amountOverdueIn(start, end)
```
becomes
```
amountInvoicedIn(range: DateRange)
amountReceivedIn(range: DateRange)
amountOverdueIn(range: DateRange)

class DateRange: start, end; includes(date)
```

## Language notes

- Records/data classes/structs/named tuples make the new type cheap.
- In JS/TS, Python (keyword args), Kotlin, etc., an "options object"/named arguments improves call-site readability but is not the same thing — the goal here is a meaningful domain type that can grow behavior.
- Check the standard library first: ranges, intervals, points, money types may already exist.

## Related

- `preserve-whole-object` (when an object already exists), `extract-class`, `replace-data-value-with-object`, `move-method`.
