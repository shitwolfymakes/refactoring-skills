---
name: add-parameter
description: "Add a parameter a function needs from its callers, migrating implementations and callers safely. Use when a function needs data it isn't given, after ruling out options that don't lengthen the parameter list."
---

# Add Parameter

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions merge it into "Change Function Declaration".*

**Problem:** A function needs more information from its caller.
**Solution:** Have callers pass the missing information in as a new parameter.

## First, consider alternatives

Long parameter lists are hard to remember and often hide data clumps. Before adding:
- Can an existing argument provide the information (or be given a method that does)?
- Should the behavior that needs the information live on the object that has it (`move-method`)?
- Do the existing parameters plus the new one form a concept (`introduce-parameter-object`, `preserve-whole-object`)?

Adding parameters is often right — just be aware of the alternatives.

## Mechanics

1. Check whether the signature is declared in a superclass/subclass/interface. If so, do these steps for each.
2. Declare a new function with the added parameter (add several at once if needed); copy the body over.
3. Build.
4. Make the old function call the new one, passing a placeholder (null/None for objects; a clearly odd value for primitives, not zero, so it's easy to spot). Skip if there are only a few callers.
5. Build and test.
6. Change each caller to the new function with a real argument. Build and test after each.
7. Remove the old function (or deprecate it if it's a published interface).
8. Build and test.

## Language notes

- Languages with default or named/keyword arguments (Python, Kotlin, C#, Swift, Scala, JS options objects) let you add an optional parameter without a parallel function — handy for migration. Remove the default afterwards if every caller should be explicit.
- IDE "Change Signature" refactorings automate this.

## Related

- Inverse: `remove-parameter`. Same mechanics: `rename-method`.
- Alternatives: `introduce-parameter-object`, `preserve-whole-object`, `move-method`.
