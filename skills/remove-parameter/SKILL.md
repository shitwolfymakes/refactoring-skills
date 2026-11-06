---
name: remove-parameter
description: "Delete a parameter the function no longer uses, migrating callers safely. Use for unused or speculative parameters; check other polymorphic implementations first."
---

# Remove Parameter

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions merge it into "Change Function Declaration".*

**Problem:** The function body no longer reads one of its parameters.
**Solution:** Remove it.

## Why

A parameter tells callers that the value matters and they must supply it. Callers must work out what to pass. An unused parameter makes work for every caller. "We might need it later" is not a reason to keep it — removing is easy, and so is adding it back.

**Polymorphic functions:** other implementations may use it. Then don't remove it. If some callers know the concrete type and go to trouble to supply a meaningless value, consider an extra function without the parameter; otherwise leave callers in blissful ignorance.

## Mechanics

1. Check whether the signature is declared in a superclass/subclass/interface, and whether any implementation uses the parameter. If one does, stop.
2. Declare a new function without the parameter (remove several at once if you like); copy the body over.
3. Build.
4. Make the old function call the new one (skip if few callers).
5. Build and test.
6. Change each caller to the new function. Build and test after each.
7. Remove the old function (or deprecate it if published).
8. Build and test.

## Language notes

- Linters/compilers often flag unused parameters (`-Wunused-parameter`, `noUnusedParameters`, `unused_variables`). Some are required by a callback/interface signature — those can't be removed; mark them as intentionally unused (`_name`).
- IDE "Change Signature" automates the call-site updates.

## Related

- Inverse: `add-parameter`. Same mechanics: `rename-method`. See also `replace-parameter-with-method`.
