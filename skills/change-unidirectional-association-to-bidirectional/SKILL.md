---
name: change-unidirectional-association-to-bidirectional
description: "Add back-pointers so two linked classes can reach each other, with one side owning updates. Use when an object must find the objects that point to it."
---

# Change Unidirectional Association to Bidirectional

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** Two classes need each other's features, but there's only a one-way link.
**Solution:** Add back-pointers, and change modifiers to update both sides.

## When to apply

A client of the referred-to class needs to get the objects referring to it. First look for another route (a query, a lookup) — computation is often an acceptable price. Add a back-pointer only when that's not practical.

Back-pointers are easy to get tangled. Write tests for the link maintenance — this is one of the rare refactorings that adds tests.

## Mechanics

1. Add a field for the back-pointer (single or collection, per the multiplicity).
2. Decide which class controls the association:
   - One-to-many: the side with the single reference (the "one" per object, e.g. `Order` owning its `customer`) controls.
   - Component/whole: the composite controls.
   - Many-to-many: either.
3. On the non-controlling side, create a helper that only adjusts its own collection/field. Name it to make its restricted use obvious (e.g. `linkedOrders()`), and limit its visibility.
4. If the existing modifier is on the controlling side, change it to update the back-pointers (remove from the old partner, add to the new).
5. If the existing modifier is on the controlled side, put the real logic in a method on the controlling side and have the existing modifier delegate to it.
6. Test both directions, including reassignment and clearing.

## Sketch

```
class Order:
    function setCustomer(newCustomer):
        if customer != null: customer.linkedOrders().remove(this)
        customer = newCustomer
        if newCustomer != null: newCustomer.linkedOrders().add(this)
```

## Language notes

- In languages with ownership/borrowing (Rust) or manual memory management, cycles need weak references or IDs/indices instead of direct pointers.
- ORMs usually model this as an inverse relation with an owning side — mirror that.
- Watch for memory leaks and serialization cycles.

## Related

- Inverse: `change-bidirectional-association-to-unidirectional`.
