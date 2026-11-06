---
name: introduce-assertion
description: "State a hidden assumption about program state as an assertion. Use when code relies on a condition that is only implied or written in a comment."
---

# Introduce Assertion

*Source: Fowler, "Refactoring" (1st ed.), Ch. 9 — Simplifying Conditional Expressions.*

**Problem:** Code silently depends on some condition about program state being true.
**Solution:** State that condition in an assertion.

## Why

Assumptions are often unstated and must be decoded from the algorithm, or are written in a comment. An assertion states them in code:
- **Communication:** readers see what the code assumes.
- **Debugging:** failures are caught near their origin.

An assertion is a condition assumed always true. A failure means **programmer error**, not a recoverable condition. Assertions must not be relied on by the rest of the system (they may be disabled in production), so signal clearly that something is an assertion.

Since assertions don't change behavior when they hold, adding one is behavior-preserving.

## Mechanics

Wherever code takes a condition for granted, write it down as an assertion. Use the language's assertion facility or a small assert helper.

## Cautions

- Don't assert everything you believe is true — only things that *need* to be true for the code to work.
- Ask: does the code still work if the assertion fails? If yes, remove the assertion; it's confusing.
- Duplicate logic in assertions smells as bad as anywhere — `extract-method` shared checks.
- Don't use assertions to validate external input (user data, API requests, files). That's error handling — use exceptions or error returns.

## Sketch

```
function shippingCost():
    // either a flat rate or a carrier must be configured
    return flatRate != NO_RATE ? flatRate : carrier.quote(weight)
```
becomes
```
function shippingCost():
    assert flatRate != NO_RATE or carrier != null
    return flatRate != NO_RATE ? flatRate : carrier.quote(weight)
```

## Language notes

- `assert` in Python is stripped with `-O`; Java's needs `-ea`; C/C++ `assert` disappears with `NDEBUG`; Rust has `assert!` (always) and `debug_assert!` (debug only). Never put side effects in an assertion.
- Type systems can encode many assumptions (non-null types, newtypes, refinement via constructors) — prefer them when they fit.

## Related

- `introduce-null-object`, `extract-method`.
