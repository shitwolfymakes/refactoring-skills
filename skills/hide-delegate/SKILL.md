---
name: hide-delegate
description: "Add delegating methods so clients stop reaching through an object to its collaborators. Use for message chains like a.getB().getC().doX()."
---

# Hide Delegate

*Source: Fowler, "Refactoring" (1st ed.), Ch. 7 — Moving Features Between Objects.*

**Problem:** A client calls a method on a delegate it got from a server object (`person.getDepartment().getManager()`).
**Solution:** Create methods on the server that hide the delegate (`person.getManager()`).

## When to apply

Encapsulation keeps each object's knowledge of the rest of the system small, so a change touches fewer places. If a client navigates through the server to its delegate, a change in the delegate can ripple to the client. A delegating method on the server confines changes to the server.

- **Message Chains** smell.
- Many clients navigate the same path.
- If all clients are hidden from the delegate, you can remove the delegate accessor from the server entirely.

## When not to

- Every new delegate feature would need a new forwarding method and the server becomes a pure pass-through → that's the Middle Man smell; see `remove-middle-man`. The right amount of hiding shifts over time; adjust in either direction.

## Mechanics

1. For each delegate method the client uses, add a forwarding method to the server.
2. Change the client to call the server. Build and test after each.
3. If no client needs the delegate any more, remove the server's accessor for it.
4. Build and test.

## Sketch

```
manager = john.getDepartment().getManager()
```
becomes
```
class Person:
    function getManager(): return department.getManager()

manager = john.getManager()
```

## Language notes

- Some languages offer delegation helpers (Kotlin `by`, Ruby `Forwardable`/`delegate`, Python `__getattr__`, decorators/macros) to cut boilerplate — use them sparingly, as they can hide the interface.
- Law of Demeter linters flag the chains this refactoring removes.

## Related

- Inverse: `remove-middle-man`.
- Alternative: `extract-method` + `move-method` on the chain's final use.
