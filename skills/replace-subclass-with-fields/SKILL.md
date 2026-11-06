---
name: replace-subclass-with-fields
description: "Replace subclasses that differ only in methods returning constants with superclass fields. Use when subclasses are too thin to justify themselves."
---

# Replace Subclass with Fields

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data. Later editions fold it into "Remove Subclass".*

**Problem:** The only difference between subclasses is methods that return fixed values.
**Solution:** Change those methods to superclass fields and eliminate the subclasses.

## When to apply

"Constant methods" (methods returning a hard-coded value per subclass) are a fine way to vary data. But a subclass with nothing except such methods doesn't earn its keep. Put the values in fields on the superclass and remove the extra complexity.

## Mechanics

1. `replace-constructor-with-factory-method` on the subclasses (one factory per variant on the superclass).
2. Replace any references to the subclasses with references to the superclass.
3. Declare immutable fields on the superclass for each constant method.
4. Add a protected superclass constructor that initializes them.
5. Make subclass constructors call it. Build and test.
6. Have the superclass version of each such method return the field; remove it from the subclasses. Build and test after each.
7. When the subclass methods are gone, `inline-method` the subclass constructor into the superclass factory. Build and test.
8. Delete the subclass. Build and test.
9. Repeat for each subclass.

## Sketch

```
class Person:  abstract getCode()
class Male   extends Person: getCode() = "M"
class Female extends Person: getCode() = "F"
```
becomes
```
class Person:
    code
    static createMale():   return Person("M")
    static createFemale(): return Person("F")
    getCode(): return code
```

## Language notes

- In languages with enums carrying data, the "variants" may be better as enum constants than as factory methods.

## Related

- Inverse direction: `replace-type-code-with-subclasses`.
- Uses: `replace-constructor-with-factory-method`, `inline-method`.
- Related: `collapse-hierarchy`.
