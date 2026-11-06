---
name: remove-middle-man
description: "Let clients call a delegate directly instead of through a forwarding class. Use when a class is mostly pass-through methods."
---

# Remove Middle Man

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A class does too much simple delegation.
**Solution:** Have clients talk to the delegate themselves.

## When to apply

`hide-delegate` has a price: every new delegate feature needs a new forwarding method on the server. When most of a class's interface is just forwarding, it's become a middle man and is in the way.

The right amount of hiding changes as the system evolves. Hide Delegate and Remove Middle Man are inverses — move back and forth as needed.

## Mechanics

1. Create (or expose) an accessor for the delegate on the server.
2. For each client use of a forwarding method: change the client to get the delegate and call it directly, then remove the forwarding method from the server.
3. Build and test after each method.

## Sketch

```
class Person:
    function getManager(): return department.getManager()
manager = john.getManager()
```
becomes
```
class Person:
    function getDepartment(): return department
manager = john.getDepartment().getManager()
```

## Related

- Inverse: `hide-delegate`.
- If the middle man adds nothing at all: `inline-method` / `inline-class`.
- If the middle man really is-a delegate: `replace-delegation-with-inheritance`.
