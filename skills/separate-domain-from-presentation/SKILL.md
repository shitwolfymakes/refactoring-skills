---
name: separate-domain-from-presentation
description: "Big refactoring: move business logic out of UI code into domain classes. Use when rules, calculations, or data access sit in views, controllers, or handlers."
---

# Separate Domain from Presentation

*Source: Fowler & Beck, "Refactoring" (1st ed.), Ch. 12 — Big Refactorings.*

**Problem:** GUI classes contain domain logic.
**Solution:** Move the domain logic into its own classes.

## Why

The core of MVC: presentation code handles only the UI; the domain objects hold every business rule and no UI code. Two complex parts become separately modifiable, testable without a UI, and you can have multiple presentations over the same logic. Many environments push you toward a two-tier "data in the database, logic in the screens" style; this refactoring gets you out.

## Mechanics

1. Give each window, screen, or view a matching domain class.
2. If there's a grid/table, create a class for the rows and hold a collection of them on the screen's domain class.
3. Examine the data on the screen:
   - Used only for UI purposes → leave it on the view.
   - Used by domain logic but not displayed → `move-method`/`move-field` it to the domain object.
   - Used by both → `duplicate-observed-data` so it exists in both places and stays in sync.
4. Go through the UI class's logic. `extract-method` to separate presentation logic from domain logic; `move-method` the domain logic to the domain object.
5. When done: views handle UI, domain objects hold all business logic. The domain objects won't be well factored yet — keep refactoring them.

## Language notes

The same applies far beyond desktop GUIs:
- **Web backends:** move business logic out of controllers/route handlers/templates into domain/service modules.
- **Frontend frameworks:** move logic out of components into plain modules, hooks/composables, stores, or view models — keep components rendering and wiring events.
- **Mobile:** MVVM/MVI — logic in view models and domain layer, not activities/view controllers.
- **CLIs:** keep argument parsing and output formatting apart from the core operations.

Plain domain modules with no UI imports are easy to unit-test — add tests as you move logic.

## Big refactorings — working style

Big refactorings take weeks or months, not minutes. Don't stop feature work to do them. Do them a little at a time, alongside other work, whenever you touch the affected code. Each small step uses ordinary refactorings and keeps the system working and tested. The team must agree on the direction, since everyone will be moving the code toward it.

## Related

- Steps: `duplicate-observed-data`, `extract-method`, `move-method`, `move-field`.
