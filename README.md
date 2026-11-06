# Refactoring skills for Claude Code

An unofficial, language-agnostic companion to Martin Fowler's *Refactoring: Improving the Design of Existing Code* (1st edition), packaged as a Claude Code plugin.

The skills paraphrase the book's catalog and credit their source chapter. They do not replace the book.

Licensed under the [MIT License](LICENSE).

## What's inside

- **`refactoring`**: the overall workflow and principles: keep behavior the same, write tests first, take small steps, don't mix new features with restructuring, decide when (not) to refactor. Also indexes every refactoring skill.
- **`code-smells`**: the 22 code smells from Chapter 3, each mapped to the refactorings that remove it.
- **72 refactoring skills**, one per catalog entry in Chapters 6–12. Each has the problem and solution, when (not) to apply it, step-by-step mechanics, notes for other languages, a pseudocode sketch, and related refactorings.

| Chapter | Topic | Skills |
|---|---|---|
| 6 | Composing Methods | 9 |
| 7 | Moving Features Between Objects | 8 |
| 8 | Organizing Data | 16 |
| 9 | Simplifying Conditional Expressions | 8 |
| 10 | Making Method Calls Simpler | 15 |
| 11 | Dealing with Generalization | 12 |
| 12 | Big Refactorings | 4 |

## Install

```
/plugin marketplace add shitwolfymakes/refactoring-skills
/plugin install refactoring@wolfy-skills
```

Skills are namespaced by the plugin, e.g. `refactoring:extract-method`.

## Usage

Ask Claude to refactor, clean up, or review code. It loads `refactoring` and `code-smells` as needed, then the specific refactoring skills. You can also invoke one directly, e.g. `/refactoring:replace-conditional-with-polymorphism`.
