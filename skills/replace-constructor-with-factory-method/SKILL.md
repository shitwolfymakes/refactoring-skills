---
name: replace-constructor-with-factory-method
description: "Create objects through a named factory function instead of the constructor. Use when creation picks a subclass, reuses instances, needs a clearer name, or does real work."
---

# Replace Constructor with Factory Method

*Source: Fowler, "Refactoring" (1st ed.), Ch. 10 — Making Method Calls Simpler. Later editions: "Replace Constructor with Factory Function".*

**Problem:** Creating an object involves more than a plain constructor call.
**Solution:** Hide the constructor behind a factory function.

## When to apply

- **Choosing a subclass:** objects created with a type code now need subclasses (`replace-type-code-with-subclasses`). A constructor can only return the class it belongs to; a factory can return any subtype.
- **Returning shared instances:** essential for `change-value-to-reference` (registries, caching, interning).
- **Naming:** constructors all share the class's name; factories can say what they make (`createEngineer()`, `fromJson()`, `ofMillis()`), which is clearer than overloading on parameter types.

## Mechanics

1. Create a factory function whose body calls the existing constructor.
2. Replace every constructor call with a call to the factory. Build and test after each.
3. Make the constructor private (or otherwise restricted).
4. Build.

## Sketch

```
Employee(type)
```
becomes
```
static function create(type): return Employee(type)
```
and later, perhaps:
```
static function create(type):
    switch type: ENGINEER: return Engineer(); SALESMAN: return Salesman(); ...
```

## Language notes

- Languages without constructors (Go `NewX()`, Rust `X::new()`) already use factory functions — the refactoring is about adding named variants or returning different implementations behind an interface.
- Python: `@classmethod` alternate constructors; JS/TS: static methods or plain module functions.
- If creation logic grows large, a separate factory or builder object may be better.

## Related

- Enables: `replace-type-code-with-subclasses`, `change-value-to-reference`, `replace-subclass-with-fields`.
