---
name: pull-up-constructor-body
description: "Move initialization shared by subclass constructors into a superclass constructor. Use when sibling constructors have mostly identical bodies."
---

# Pull Up Constructor Body

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Subclass constructors have mostly identical bodies.
**Solution:** Create a superclass constructor and call it from the subclass constructors.

## Why

Constructors aren't ordinary methods: they aren't inherited, so `pull-up-method` doesn't apply. When the common behavior *is* construction, put it in a superclass constructor that subclasses call. If this gets complex, consider `replace-constructor-with-factory-method` instead.

## Mechanics

1. Define a superclass constructor.
2. Move the common code from the start of the subclass constructors into it (this may be all of it). Try to rearrange so common code comes first.
3. Call the superclass constructor as the first statement of each subclass constructor (if all code was common, that's the only line).
4. Build and test.
5. Common code that must run later (after subclass-specific setup): `extract-method` it and `pull-up-method` it, then call it from the subclass constructors.

## Sketch

```
class Manager extends Employee:
    constructor(name, id, grade):
        this.name = name; this.id = id; this.grade = grade
```
becomes
```
class Employee:
    constructor(name, id): this.name = name; this.id = id
class Manager extends Employee:
    constructor(name, id, grade): super(name, id); this.grade = grade
```

## Language notes

- Beware of calling overridable methods from a superclass constructor — the subclass isn't fully initialized yet in many languages (Java, C#, C++ behave differently from each other here).
- Go struct embedding / Rust: a shared constructor function for the embedded/common part that each variant's constructor calls.

## Related

- `pull-up-method`, `extract-method`, `replace-constructor-with-factory-method`, `extract-superclass`.
