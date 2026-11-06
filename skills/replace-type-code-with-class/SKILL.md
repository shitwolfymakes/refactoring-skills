---
name: replace-type-code-with-class
description: "Replace a type code that doesn't drive behavior with an enum or dedicated type the type checker enforces. Use for status or category codes stored as ints or strings."
---

# Replace Type Code with Class

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** A class carries a numeric or string code whose value never changes how the class behaves.
**Solution:** Replace the code with a new class (a closed set of instances).

## When to apply

Symbolic names for integer/string codes are only aliases; the type system still sees `int`/`string`, so any number can be passed where a code is expected. With a dedicated type, the compiler or type checker rejects anything that isn't a valid code.

**Choose the right type-code refactoring:**
- Code is pure data (no conditionals on it) → **this refactoring**.
- Code drives behavior (switch/if chains) and is fixed per object → `replace-type-code-with-subclasses`.
- Code drives behavior but changes at runtime, or the class is already subclassed → `replace-type-code-with-state-strategy`.

Even without switches, watch for behavior that belongs on the new type (`move-method`).

## Mechanics

1. Create a class for the type code: a field for the code value and its getter, static instances for each allowed value, and a static lookup from a raw code to the instance.
2. Make the source class use the new class internally while keeping its code-based interface (static code constants now come from the new class).
3. Build and test. (The new class can now validate codes at runtime.)
4. For each source method that takes or returns a code, add a parallel method that takes/returns the new type. Consider `rename-method` on old accessors to make old-code usage obvious.
5. Change clients one by one to the new interface. Build and test after each (some groups must change together).
6. Remove the old code-based interface and code constants.
7. Build and test.

## Language notes

- Modern **enums** do this directly: Java/Kotlin/Swift/Rust/C# enums, Python `enum.Enum`, TS string-literal unions or `enum`. Prefer the language's enum where it exists.
- Rich enums (Java, Kotlin, Swift, Rust) can hold data and methods — handy for moving behavior in.
- Map external codes (database, wire format) to the enum at the boundary.

## Related

- Alternatives: `replace-type-code-with-subclasses`, `replace-type-code-with-state-strategy`.
- Generalization: `replace-data-value-with-object`. See also `replace-magic-number-with-symbolic-constant`.
