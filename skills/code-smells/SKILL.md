---
name: code-smells
description: "Code smells (Beck & Fowler, Refactoring Ch. 3) mapped to the refactorings that fix them. Use when reviewing code quality or deciding what to refactor: duplication, long functions, large classes, feature envy, switch-on-type, and the like."
---

# Code smells → refactorings

*Source: Kent Beck & Martin Fowler, "Refactoring" (1st ed.), Ch. 3 — Bad Smells in Code.*

Smells indicate that a refactoring *may* help; they are not hard rules. No metric beats informed judgment about how long is too long or how many fields is too many. Identify the smell, then consult the suggested refactoring skills (follow the `refactoring` skill's workflow: tests first, small steps).

## Duplicated Code
The same code shape shows up in several places. This is the most important smell to fix.
- Same expression in two functions of one class/module → `extract-method`, call it from both.
- Same code in sibling subclasses → `extract-method` in each, then `pull-up-method`. Similar but not identical → `extract-method` to separate same from different, then maybe `form-template-method`. Same job, different algorithms → `substitute-algorithm` to the clearer one.
- In unrelated classes → `extract-class` and use the new component from both, or decide which single place owns it (maybe a third class).

## Long Method
Long functions are harder to understand. The key is the distance between *what* code does and *how*.
- 99% of the time: `extract-method`. Look for comments, conditionals, and loops as extraction points; extract even one line if it needs explaining.
- Temps and parameters getting in the way → `replace-temp-with-query`, `introduce-parameter-object`, `preserve-whole-object`.
- Still too tangled → `replace-method-with-method-object`.
- Conditionals → `decompose-conditional`. Loops → extract the loop and its body.

## Large Class
Too many fields, too much code; duplication follows.
- Group fields that belong together (shared prefixes/suffixes, e.g. `depositAmount`/`depositCurrency`) → `extract-class`, or `extract-subclass` if the group is a variation.
- Fields not always used → repeated `extract-class` / `extract-subclass`.
- Duplicated code within it → collapse into small shared functions first.
- See how clients use it → `extract-interface` per usage to reveal split lines.
- A UI class holding domain logic → `duplicate-observed-data`, `separate-domain-from-presentation`.

## Long Parameter List
Hard to understand, inconsistent to use, constantly changing.
- Value obtainable from an object you already have → `replace-parameter-with-method`.
- Several values from one object → `preserve-whole-object`.
- Several values with no owning object → `introduce-parameter-object`.
- Exception: when you deliberately avoid a dependency from callee to the larger object. If that list keeps growing, rethink dependencies.

## Divergent Change
One class keeps being edited for unrelated reasons (for example, some methods change whenever a storage backend is added, others whenever a new product type is).
- Gather everything that changes for one cause → `extract-class`.

## Shotgun Surgery
One kind of change requires many small edits across many classes.
- Bring the changes into one place → `move-method`, `move-field`; `inline-class` to gather scattered behavior.
- Goal: each kind of change touches exactly one class or module.

## Feature Envy
A function more interested in another class's data than its own.
- `move-method` it there; if only part envies, `extract-method` that part and move it.
- Uses several classes → move it to the one with most of the data (split with `extract-method` first).
- Deliberate exceptions: Strategy/Visitor patterns that separate behavior to fight Divergent Change.

## Data Clumps
The same few data items appear together in fields and parameter lists.
- Fields → `extract-class`. Parameters → `introduce-parameter-object` / `preserve-whole-object`.
- Test: delete one value — do the others still make sense? If not, an object wants to be born. Then look for Feature Envy to move behavior onto it.

## Primitive Obsession
Using primitives (numbers, strings, arrays) for domain concepts like money, ranges, phone numbers, ZIP codes, type codes.
- Single values → `replace-data-value-with-object`.
- Type code without behavior → `replace-type-code-with-class`; with behavior → `replace-type-code-with-subclasses` / `replace-type-code-with-state-strategy`.
- Groups of fields → `extract-class`; in parameter lists → `introduce-parameter-object`.
- Arrays used as records → `replace-array-with-object`.

## Switch Statements
The same type-based switch/if-chain scattered in several places; each new case means finding them all.
- `extract-method` the switch, `move-method` it to the class holding the type code, then `replace-type-code-with-subclasses` or `replace-type-code-with-state-strategy`, then `replace-conditional-with-polymorphism`.
- Few cases, one function, unlikely to change → polymorphism is overkill; `replace-parameter-with-explicit-methods` may suit.
- One case is null → `introduce-null-object`.
- (In languages with exhaustive pattern matching over sealed/sum types, a single match is often fine; *repeated* matches on the same type are the smell.)

## Parallel Inheritance Hierarchies
Each new subclass in one hierarchy requires one in another (matching name prefixes).
- Make instances of one hierarchy refer to the other, then `move-method` / `move-field` until the referring hierarchy disappears.

## Lazy Class
A class (or module) not doing enough to justify itself.
- Subclasses not doing enough → `collapse-hierarchy`. Near-useless components → `inline-class`.

## Speculative Generality
Hooks and abstractions for "someday" that nothing uses.
- Idle abstract classes → `collapse-hierarchy`. Unnecessary delegation → `inline-class`. Unused parameters → `remove-parameter`. Oddly abstract names → `rename-method`.
- Code whose only users are tests → delete it and its tests.

## Temporary Field
A field set only in certain circumstances (often several fields used only during one algorithm).
- `extract-class` for the fields and the code using them (a method object, see `replace-method-with-method-object`).
- `introduce-null-object` for the case when the fields aren't valid.

## Message Chains
`a.getB().getC().getD()…` — clients coupled to the navigation structure.
- `hide-delegate` at points in the chain (not everywhere, or you get Middle Men).
- Better: see what the end result is used for; `extract-method` that code and `move-method` it down the chain.

## Middle Man
Half a class's methods just delegate.
- `remove-middle-man`. A few trivial ones → `inline-method`. If it adds some behavior → `replace-delegation-with-inheritance`.

## Inappropriate Intimacy
Classes digging into each other's private parts.
- `move-method`, `move-field` to separate; `change-bidirectional-association-to-unidirectional`; `extract-class` for shared interests; `hide-delegate` to put a go-between in place.
- Subclass too intimate with parent → `replace-inheritance-with-delegation`.

## Alternative Classes with Different Interfaces
Classes doing the same thing with different signatures.
- `rename-method` to align; `move-method` until protocols match; `extract-superclass` if this duplicates code.

## Incomplete Library Class
A library type lacks something you need and you can't change it.
- One or two methods → `introduce-foreign-method`. Many → `introduce-local-extension`.

## Data Class
Only fields and accessors; others manipulate it in detail.
- Public fields → `encapsulate-field`. Collections → `encapsulate-collection`. Immutable fields → `remove-setting-method`.
- Move behavior in from its users: `move-method` (with `extract-method` for partial pieces). Then `hide-method` on accessors no longer needed.

## Refused Bequest
Subclasses use little of what they inherit.
- Usually a faint smell — leave it unless it causes confusion. If it does: create a sibling and `push-down-method` / `push-down-field` the unused parts.
- Strong smell: subclass reuses behavior but refuses the superclass's *interface* → `replace-inheritance-with-delegation`.

## Comments (as deodorant)
Comments aren't bad, but thick comments often cover bad code. Refactor first; often the comment becomes superfluous.
- Comment explains what a block does → `extract-method`.
- Comment explains what a function does → `rename-method`.
- Comment states required state → `introduce-assertion`.
- Good comments: *why* you did something, and where you're unsure.
