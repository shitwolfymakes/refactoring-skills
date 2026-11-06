---
name: tease-apart-inheritance
description: "Big refactoring: split a hierarchy doing two jobs into two hierarchies linked by delegation. Use when subclasses multiply combinatorially, e.g. every type has a Tabular variant."
---

# Tease Apart Inheritance

*Source: Fowler & Beck, "Refactoring" (1st ed.), Ch. 12 — Big Refactorings.*

**Problem:** An inheritance hierarchy is doing two jobs at once.
**Solution:** Split it into two hierarchies, one holding a reference to the other.

## Why

Hierarchies get tangled gradually: a subclass is added for one concern, then the same concern is added at another level, and the classes start to multiply. The result is duplicated code, one policy scattered across many classes, and a design you can't describe in one sentence.

**Telltale sign:** every class at one level has subclasses whose names begin with the same adjective (`TabularActiveDeal`, `TabularPassiveDeal`). That's a second dimension squeezed into one hierarchy.

## Mechanics

1. Identify the jobs the hierarchy is doing. Draw a grid with one axis per job (more than two dimensions → repeat this refactoring, one dimension at a time).
2. Pick the job that stays in the existing hierarchy; the other one moves out.
3. Use `extract-class` on the shared superclass to give the secondary job its own object, held in a field.
4. Create subclasses of the extracted class corresponding to each subclass in the original hierarchy for that job. Initialize the field to the appropriate one.
5. `move-method` the subsidiary-job behavior from each original subclass into the corresponding extracted subclass.
6. Delete each original subclass once it is empty.
7. Continue until all such subclasses are gone. Look for further cleanup (`pull-up-method`, `pull-up-field`) in both hierarchies.

## Sketch

```
Deal ─ ActiveDeal ─ TabularActiveDeal
     └ PassiveDeal ─ TabularPassiveDeal
```
becomes
```
Deal (has a PresentationStyle) ─ ActiveDeal, PassiveDeal
PresentationStyle ─ SingleStyle, TabularStyle
```

## Language notes

- This is the Bridge pattern. In languages without inheritance, it's simply two independent interfaces/traits composed in a struct.

## Big refactorings — working style

Big refactorings take weeks or months, not minutes. Don't stop feature work to do them. Do them a little at a time, alongside other work, whenever you touch the affected code. Each small step uses ordinary refactorings and keeps the system working and tested. The team must agree on the direction, since everyone will be moving the code toward it.

## Related

- Steps: `extract-class`, `move-method`, `pull-up-method`, `pull-up-field`.
- `replace-type-code-with-state-strategy`, `replace-inheritance-with-delegation`.
