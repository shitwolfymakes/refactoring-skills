---
name: replace-inheritance-with-delegation
description: "Turn a subclass into a class that holds its former superclass and forwards only what it supports. Use for Refused Bequest or when is-a doesn't really hold."
---

# Replace Inheritance with Delegation

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization. Later editions: "Replace Superclass with Delegate".*

**Problem:** A subclass uses only part of its superclass's interface, or doesn't want to inherit its data.
**Solution:** Create a field for the superclass, delegate to it, and remove the subclassing.

## Why

You start by inheriting, then find many superclass operations aren't true of the subclass, it inherits inappropriate data, or protected superclass methods make no sense for it. Living with it "by convention" means the code says one thing while you intend another. Delegation makes the partial use explicit and lets you choose what to expose. The cost — boring forwarding methods — is low because they're too simple to go wrong.

## Mechanics

1. Give the subclass a field holding a superclass instance, initially set to `this`/`self`.
2. Rewrite the subclass's own methods one at a time to go through that field rather than inherited members. Build and test after each.
   - Methods that call `super` for something defined on the subclass can't be converted yet (they'd recurse) — convert them after inheritance is broken.
3. Remove the inheritance declaration; initialize the delegate field to a new instance of the former superclass.
4. For each former superclass method that clients use, add a simple forwarding method.
5. Build and test.

## Sketch

```
class Stack extends Vector:
    push(x): add(x)
    pop():   return removeLast()
```
becomes
```
class Stack:
    items = Vector()
    push(x): items.add(x)
    pop():   return items.removeLast()
    isEmpty(): return items.isEmpty()
    size():    return items.size()
```

## Language notes

- Clients that relied on the subclass being substitutable for the superclass type will break — check for uses of the class as its former supertype (that's often the point: it shouldn't have been).
- Delegation helpers reduce forwarding boilerplate: Kotlin `by`, Go struct embedding (exposes *all* methods — use a named field to control exposure), Ruby `Forwardable`, Lombok `@Delegate`.
- If a common interface is still needed, `extract-interface` and implement it on the new class.

## Related

- Inverse: `replace-delegation-with-inheritance`.
- Alternatives when designing: `extract-class` instead of `extract-subclass`.
- Mechanics resemble `introduce-local-extension` (wrapper variant).
