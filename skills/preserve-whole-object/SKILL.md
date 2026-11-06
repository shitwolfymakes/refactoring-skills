---
name: preserve-whole-object
description: "Pass an object instead of several values pulled out of it. Use when multiple arguments come from the same source object."
---

# Preserve Whole Object

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** You get several values from an object and pass them as parameters to a call.
**Solution:** Send the whole object instead.

## Why

- If the callee later needs more data from that object, every call site must change. With the whole object, the callee asks for what it needs.
- Shorter parameter lists are easier to read; callers and callees don't have to remember positions.
- The callee can use the object's other methods instead of recomputing derived values (reducing duplication).

## When not to

- It would create a dependency from the callee to the object's type that harms your dependency structure (e.g. across layers or packages). Passing values creates no such dependency. This is the main reason not to do it.
- (Passing one value vs one object is not, by itself, a reason — they're equivalent for clarity.)

If the callee uses lots of values from the object, maybe the callee belongs *on* that object — consider `move-method`. If the values don't come from a single existing object, `introduce-parameter-object`. If the caller passes several of its own fields, it can pass itself (`this`/`self`), given suitable accessors and acceptable dependency.

## Mechanics

1. Add a parameter for the whole object. Build and test.
2. Decide which parameters can be obtained from it.
3. Take one such parameter: replace its uses in the body with calls on the whole object, then delete the parameter. Build and test.
4. Repeat for each.
5. Remove the caller code that extracted the deleted values (unless used elsewhere). Build and test.

## Sketch

```
low = daysTempRange().low
high = daysTempRange().high
withinPlan = plan.withinRange(low, high)
```
becomes
```
withinPlan = plan.withinRange(daysTempRange())
```

## Language notes

- In languages with interfaces/protocols/structural typing, depend on a narrow interface the object satisfies (e.g. `HasRange`) to avoid coupling to the concrete type.

## Related

- `introduce-parameter-object`, `move-method`, `replace-parameter-with-method`.
