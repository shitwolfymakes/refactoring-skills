---
name: form-template-method
description: "Make sibling subclass methods that run the same steps in the same order identical, then pull them up as a template method. Use to remove structural duplication across subclasses."
---

# Form Template Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 11 — Dealing with Generalization.*

**Problem:** Two subclasses each have a method that follows the same sequence of steps, but the details of each step differ.
**Solution:** Put each step into a method with the same signature, so the original methods become identical; then pull them up.

## Why

You want to remove all the duplication you can while keeping the essential differences. Move the shared *sequence* to the superclass and let polymorphism vary the individual steps — the Template Method pattern.

## Mechanics

1. Decompose the methods (`extract-method`) so each extracted piece is either identical across subclasses or completely different.
2. `pull-up-method` the identical pieces into the superclass.
3. For the different pieces, `rename-method` so each step has the same signature in every subclass. Now the original methods are textually identical — same calls, different implementations. Build and test after each signature change.
4. `pull-up-method` one of the original methods. Declare the varying steps as abstract methods on the superclass. Build and test.
5. Remove the other subclasses' copies of the original method, building and testing after each.

## Sketch

```
class RetailOrder:    total(): sub = qty * price;       fee = sub * SHIPPING_RATE; return sub + fee
class WholesaleOrder: total(): sub = qty * price * 0.8; fee = FLAT_SHIPPING;       return sub + fee
```
becomes
```
abstract class Order:
    total(): return subtotal() + shippingFee()
    abstract subtotal(); abstract shippingFee()
class RetailOrder:    subtotal() …; shippingFee() …
class WholesaleOrder: subtotal() …; shippingFee() …
```

## Language notes

- Without inheritance, or when you prefer composition: make the template a function that takes the varying steps as parameters (functions/closures, a strategy object, or a trait with default method implementing the template).
- Keep the number of hook steps small; many hooks make the flow hard to follow.

## Related

- Uses: `extract-method`, `rename-method`, `pull-up-method`.
- Part of: `extract-superclass`.
