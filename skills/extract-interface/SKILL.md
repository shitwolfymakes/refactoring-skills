---
name: extract-interface
description: "Declare a subset of a class's operations as an interface and make clients depend on it. Use when clients use only part of a class, classes share a role, or you need alternative implementations or test doubles."
---

# Extract Interface

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Clients rely on only one slice of a class's operations, or two classes overlap in part of what they offer.
**Solution:** Extract that subset into an interface.

## Why

Clients use classes in different ways: sometimes the whole class, sometimes just a subset of its responsibilities, and sometimes they need any object able to respond to a given set of calls. Making that subset explicit shows how responsibilities divide and what a new implementation must provide.

Good uses:
- A class plays **distinct roles** in different contexts — one interface per role.
- Describing a class's **outbound** needs (what it requires from its server), so other servers can be plugged in later.
- Allowing substitute implementations (including fakes for tests).

**vs. `extract-superclass`:** an interface shares only *signatures*, not code. That can leave duplicated implementations — reduce it by `extract-class` into a shared component that implementations delegate to. With substantial common behavior, a superclass is simpler (but you only get one).

## Mechanics

1. Create an empty interface.
2. Declare the common operations in it.
3. Mark the classes that provide those operations as implementing it.
4. Update clients' declared types to refer to the interface.
5. Build and test.

## Language notes

- Java/C#/TS `interface`, Swift/ObjC `protocol`, Rust `trait`, Kotlin `interface`, Scala `trait`, C++ abstract base class, Python `typing.Protocol` or `abc.ABC`, Haskell typeclass.
- **Go** interfaces are satisfied implicitly — idiomatically, define small interfaces on the *consumer* side; no change to the implementing type is needed.
- **Dynamic languages** with duck typing don't need it for dispatch, but a declared protocol still documents the role and helps type checkers.
- Don't extract an interface with a single implementation just in case (Speculative Generality) — do it when there's a real second client role or implementation.

## Related

- `extract-superclass`, `extract-class`, `inline-class` (may extract an interface first).
