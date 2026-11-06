---
name: convert-procedural-design-to-objects
description: "Big refactoring: turn procedural logic over passive data records into objects that own their behavior. Use for manager/util classes holding all logic beside accessor-only data classes."
---

# Convert Procedural Design to Objects

*Source: Fowler & Beck, "Refactoring" (1st ed.), Ch. 12 — Big Refactorings.*

**Problem:** Code is written in a procedural style.
**Solution:** Make the records into classes, split the procedures apart, and give each piece to the class whose data it uses.

## Why

Using an OO language doesn't make code object-oriented. The usual picture: one class with little state and long procedures, operating on passive records that only have getters and setters. Behavior and the data it uses live apart, so changes scatter.

Not every behavior-only object is bad — small strategy objects that exist for flexibility are fine. The target is the big procedural blob.

## Mechanics

1. Give each record type a simple class with accessors (`replace-record-with-data-class`). For a relational database, start with one per table.
2. Put all the procedural code into a single class (static functions or one instance).
3. Break down each long procedure with `extract-method` and related refactorings (`replace-temp-with-query`, `replace-method-with-method-object`…). As pieces emerge, `move-method` each to the data class it works on.
4. Continue until the original class has no behavior left. If it was purely procedural, delete it.

## Sketch

```
class OrderCalculator: determinePrice(order); determineTaxes(order)
class Order:           lines (accessors only)
class OrderLine:       qty, product (accessors only)
```
becomes
```
class Order:     getPrice(); getTaxes()
class OrderLine: getPrice(); getTaxes()
```

## Language notes

- In a functional or data-oriented language, the analogous goal is not necessarily objects: it's grouping functions with the data types they operate on (modules per type), making illegal states unrepresentable, and splitting long procedures into small pure functions. Apply the decomposition steps; adapt the "move onto the object" step to "move into the type's module".

## Big refactorings — working style

Big refactorings take weeks or months, not minutes. Don't stop feature work to do them. Do them a little at a time, alongside other work, whenever you touch the affected code. Each small step uses ordinary refactorings and keeps the system working and tested. The team must agree on the direction, since everyone will be moving the code toward it.

## Related

- Steps: `replace-record-with-data-class`, `extract-method`, `move-method`.
