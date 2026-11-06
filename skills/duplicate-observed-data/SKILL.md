---
name: duplicate-observed-data
description: "Copy domain data that lives only in UI widgets into a domain object kept in sync by observer or binding. Use when untangling business logic from UI code."
---

# Duplicate Observed Data

*Source: Fowler, "Refactoring" (1st ed.), Ch. 8 — Organizing Data.*

**Problem:** Data that domain logic depends on lives only inside a UI widget.
**Solution:** Keep a copy in a domain object, and sync the two with an observer or data binding.

## When to apply

A layered system separates UI code from business logic: you may want several UIs over the same logic, the UI gets too complex otherwise, domain code is easier to evolve and test alone, and different people may own each part. Behavior is easy to move; data isn't — controls need their own copy. So you duplicate it and provide synchronization.

This is a building block of `separate-domain-from-presentation`.

## Mechanics

1. Have the UI class observe the domain class. Create the domain class if needed, and give the presentation a reference to it.
2. `self-encapsulate-field` on the domain data within the UI class (accessors read/write the control).
3. Build and test.
4. In the event handler, call the setter with the control's current value (read the control directly, not via the getter). This looks redundant but routes all updates through the setter.
   - Make sure tests actually trigger the event-handling path.
5. Build and test.
6. Define the data and accessors in the domain class. Its setter must trigger observer notification. Keep the same data type as the UI (often a string) for now; convert later.
7. Redirect the UI accessors to read/write the domain field.
8. Make the observer's update method copy the domain value into the control.
9. Build and test.

## Language notes

The 1999 mechanics use the classic Observer pattern. Modern equivalents:
- **Data binding / MVVM** (WPF, SwiftUI `@Published`/`@Observable`, Android LiveData/StateFlow, Angular, Vue reactivity).
- **Unidirectional data flow** (React state/stores, Redux, Elm): the "domain object" is the store; the view renders from it and dispatches events to it.
- Server-side templates: build a view model from the domain instead of reading values back from the view.

The principle is the same: one source of truth in the domain, the UI mirrors it.

## Related

- Larger version: `separate-domain-from-presentation`.
- Uses: `self-encapsulate-field`.
