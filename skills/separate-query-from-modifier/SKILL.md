---
name: separate-query-from-modifier
description: "Split a function that returns a value and changes state into a pure query and a separate modifier (command-query separation). Use when a getter has side effects."
---

# Separate Query from Modifier

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A function both hands back a value and has side effects on state.
**Solution:** Split it in two: a query that only reads, and a modifier that only changes.

## Why

A function that only returns a value, with no visible side effects, is easy to work with: you can call it any number of times and move calls around safely. Clearly signal the difference: a good rule is that any function that returns a value should have no *observable* side effects (Meyer's command-query separation).

"Observable" matters: caching a query result in a field changes internal state but not anything a caller can observe, so it's fine.

## Mechanics

1. Create a query returning the same value as the original. (If the original returns a temp, look at where the temp is assigned.)
2. Make the original return the result of calling the query: every `return` becomes `return newQuery()`. Remove any temp that just captured the return value.
3. Build and test.
4. For each caller: replace the single call with a call to the modifier (original) followed by a call to the query. Build and test after each.
5. Make the original return nothing (void/unit) and remove its return expressions. Rename it if needed.
6. Remove duplication between the two (often the modifier can use the query).

## Sketch

```
function foundMiscreantAndAlert(people):
    for p in people:
        if p in {"Don", "John"}: sendAlert(); return p
    return ""
```
becomes
```
function foundPerson(people):
    for p in people: if p in {"Don", "John"}: return p
    return ""

function sendAlertIfMiscreant(people):
    if foundPerson(people) != "": sendAlert()
```

## Concurrency note

If the caller needs the query and modification to happen atomically (e.g. "pop" or "get-and-increment" under concurrent access), keep a combined operation for that purpose, implemented on top of the separate query and modifier with appropriate synchronization.

## Related

- Enables: `replace-temp-with-query`, `consolidate-conditional-expression`.
