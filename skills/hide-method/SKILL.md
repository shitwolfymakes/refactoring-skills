---
name: hide-method
description: "Narrow a method's or export's visibility as far as possible. Use when nothing outside its class or module calls it."
---

# Hide Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A method isn't used by any other class.
**Solution:** Make it private (as private as possible).

## Why

Refactoring changes visibility decisions over time. It's easy to notice when something needs to be *more* visible; harder to notice when it's *too* visible. A narrow interface is easier to understand and to change.

Common case: a class starts as a data holder with public getters/setters. As behavior moves in, many accessors are no longer needed publicly — hide them (or remove them if you use direct field access internally).

## Mechanics

1. Check regularly for methods that can be more private: use linters/static analysis (unused-export or "can be private" inspections), periodic manual checks, and check whenever you remove a call from another class. Look especially at setters.
2. Give each method the narrowest visibility that still compiles and passes tests.
3. Build after a group of changes; the compiler/type checker will catch any that were still needed.
4. Run tests (reflection, dynamic dispatch, templates, and tests themselves may use the method in ways the compiler can't see).

## Language notes

- Module systems: stop exporting it (`export`, `pub`, `__all__`, capitalized names in Go, `internal` in C#/Kotlin).
- Python: a leading underscore signals non-public.
- Don't widen visibility just for tests; test through the public interface or reconsider the design (often the hidden logic wants to be its own class).

## Related

- `remove-setting-method`, `encapsulate-field`.
