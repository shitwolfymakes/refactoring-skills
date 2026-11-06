---
name: rename-method
description: "Give a function a name that reveals its intent, updating all callers and implementations (Change Function Declaration). Use when a name is cryptic, misleading, or stale."
---

# Rename Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions merge it into "Change Function Declaration".*

**Problem:** The name of a method doesn't reveal its purpose.
**Solution:** Change the name.

## Why

Small, well-factored functions only help if their names say what they do; otherwise readers go on a "merry dance" through them. A good trick: think of the comment you'd write for the function and turn it into the name.

You won't get names right the first time. Don't leave a bad name because "it's only a name" — code is for humans first. Good naming is a core programming skill. The same goes for the rest of the signature: reorder parameters if it clarifies (`add-parameter`, `remove-parameter`).

## Mechanics

1. Check whether the signature is declared in a superclass/subclass/interface. If so, do these steps for every declaration and implementation.
2. Declare a new function with the new name; copy the body over and adjust.
3. Build.
4. Make the old function call the new one. (Skip this step if there are only a few callers.)
5. Build and test.
6. Change each reference to the old name to the new one. Build and test after each (or use an automated rename).
7. Remove the old function. If it's part of a published interface you can't change, keep it, mark it deprecated, and keep it forwarding.
8. Build and test.

## Language notes

- Prefer the IDE/language server's automated rename — it handles overrides, imports, and references. Still check reflection, string-based lookups, serialization names, config files, templates, and dynamic dispatch (`getattr`, `send`, `method_missing`) that tools can't see.
- For public libraries, use the language's deprecation mechanism (`@Deprecated`, `[Obsolete]`, `#[deprecated]`, JSDoc `@deprecated`, `warnings.warn(DeprecationWarning)`).
- The same discipline applies to renaming variables, fields, classes, and modules.

## Related

- Same mechanics: `add-parameter`, `remove-parameter`.
