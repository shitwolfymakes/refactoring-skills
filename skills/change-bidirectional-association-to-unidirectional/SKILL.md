---
name: change-bidirectional-association-to-unidirectional
description: "Drop the unneeded end of a two-way object link. Use when back-pointers add coupling or upkeep and one side no longer navigates to the other."
---

# Change Bidirectional Association to Unidirectional

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** Two classes point at each other, yet one of them has stopped needing to reach the other.
**Solution:** Drop the unneeded end.

## Why

Bidirectional associations cost: keeping both links correct on create/delete, bugs from inconsistency, orphaned objects that should be dead but are still referenced, and mutual dependency between the classes (and their packages). Use them when needed — drop them when they stop pulling their weight.

## Mechanics

1. Examine every reader of the field you want to remove (and callers of those readers) to see whether removal is feasible.
   - Can the other object be determined without the pointer (e.g. by query)? Then `substitute-algorithm` on the getter keeps clients working.
   - Could the object be passed as an argument to the methods that use the field?
2. If clients need the getter: `self-encapsulate-field`, `substitute-algorithm` the getter to compute the value another way, build and test.
3. If clients don't need the getter: change each user to obtain the object another way. Build and test after each.
4. When no readers remain, remove all updates to the field:
   - If many places assign it, `self-encapsulate-field` so all go through one setter; build and test.
   - Empty the setter's body; build and test.
   - Remove the field, setter, and calls.
5. Build and test.

## Related

- Inverse: `change-unidirectional-association-to-bidirectional`.
- Uses: `self-encapsulate-field`, `substitute-algorithm`.
