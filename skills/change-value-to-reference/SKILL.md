---
name: change-value-to-reference
description: "Replace duplicate equal copies of an entity with one shared instance from a registry or repository. Use when updates to an entity must be seen by everything that refers to it."
---

# Change Value to Reference

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** Many equal copies of one entity exist where a single shared instance is wanted.
**Solution:** Make it a reference object: one instance per real-world thing, shared by everything that refers to it.

## Reference vs value objects

- **Reference objects** (customer, account): each stands for one real-world thing; equality is identity.
- **Value objects** (date, money): defined entirely by their data; copies are fine; equality is by value (override equality *and* hashing).

Switch to a reference when something that started as a simple value needs changeable data, and the changes must be seen by everyone who refers to it.

## Mechanics

1. `replace-constructor-with-factory-method` so creation goes through one place.
2. Build and test.
3. Decide what provides access to the instances: a registry/repository/identity map, a static dictionary, a dependency-injected service. There may be several access points.
4. Decide whether instances are pre-created (then ensure they're loaded before use) or created on demand.
5. Change the factory to return the shared instance. If pre-created, decide how to handle a request for a non-existent one (error, null object, create). Consider renaming the factory (`rename-method`) to signal it returns an existing object, e.g. `getNamed`, `find`.
6. Build and test.

## Language notes

- In persistence-backed systems this is the identity map / repository pattern; the ORM may already provide it.
- Beware of global registries: prefer an injected repository for testability, and think about concurrency and lifecycle (memory leaks from never-evicted entries).

## Related

- Inverse: `change-reference-to-value`.
- Prerequisite: `replace-constructor-with-factory-method`.
- Often follows: `replace-data-value-with-object`.
