---
name: replace-error-code-with-exception
description: "Signal errors with an exception or typed Result instead of special return values. Use when callers must check magic codes like -1 or null and errors get ignored."
---

# Replace Error Code with Exception

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler.*

**Problem:** A function signals failure through a special return value.
**Solution:** Throw an exception instead (or use the language's idiomatic error channel).

## Why

The code that detects an error often isn't the code that knows what to do about it, so the error must travel up the call chain. Special return codes mix error signaling with normal results, are easy to ignore, and must be checked and passed along by hand. Exceptions separate normal processing from error processing, which makes code easier to understand.

## Mechanics

1. Decide on the kind of error:
   - If callers are expected to check the condition **before** calling (a precondition), use an unchecked/runtime error — a violation is a programmer error.
   - If it's a genuine, expected failure the caller must handle, use a checked exception (in Java) or a specific, documented error type. Reuse an existing one or create one.
2. Adjust callers:
   - Unchecked: make each caller check the condition before calling. Build and test after each.
   - Checked/expected: wrap calls in error handling (`try`/`catch`, etc.).
3. Change the function's signature/implementation to raise the error instead of returning the code.
4. Build and test.

**Gradual version for many callers:**
1. Decide the error kind.
2. Create a new function that raises the error.
3. Make the old function call the new one and translate back to a code.
4. Build and test.
5. Move callers to the new function one at a time, building and testing after each.
6. Delete the old function.

## Sketch

```
function withdraw(amount):
    if amount > balance: return -1
    balance -= amount
    return 0
```
becomes
```
function withdraw(amount):
    if amount > balance: raise InsufficientFunds(amount, balance)
    balance -= amount
```

## Language notes — use the idiomatic error channel

- **Exceptions:** Java, C#, Python, JS/TS, Ruby, Kotlin, C++.
- **Result/Either types:** Rust `Result<T, E>` + `?`, Haskell/Scala `Either`, Swift `throws` or `Result`, F#/OCaml `result`. Here the refactoring is "replace sentinel/magic value with a typed error" rather than introducing exceptions.
- **Go:** return `(value, error)` and wrap errors; don't panic for expected failures.
- **C:** keep return codes, but use distinct named codes plus out-parameters rather than overloading the result value.
- Never use exceptions for normal control flow — see `replace-exception-with-test`.

## Related

- Inverse direction for predictable conditions: `replace-exception-with-test`.
- `replace-nested-conditional-with-guard-clauses`.
