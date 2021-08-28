---
title: TypeScript
author: vwkd
index: 2
tags:
  - languages
  - typescript
---

- statically typed superset of JS
- elevated development experience with powerful tooling around static type-checking
- zero cost since syntax is otherwise equal to JS, transpiles to plain JS



## Types

- turns dynamically typed language back into statically typed language, i.e. variable can hold only specified data type
- takes JS type system and writes it down, static formalisation of JS's dynamic type system
- enforces the subset of JS that makes sense, not the one that is syntactically valid
- but much more powerful types than in any other classical language
  - implicit inference hides a lot of typing
  - types are narrowed down based on control flow, type guards, assignments, etc.
  - conditional types, everything is a type
- doesn't force you to strongly type everything, no binary choice, can dial it up gradually, the more types the better checking
- can use types to build documentation, no need to annotate just for documentation, avoids extra layer of complexity and potential errors



## Tooling

- types is what allows for powerful tooling, because values have different functionality based on their types, e.g. control flow, function arguments, available methods (a `toString()` method but not necessarily a `toUpperCase()` method), etc.
- needs to restrict values so can reason about behavior during development
- powerful tooling, e.g. error checking, statement completion, safe refactoring, code navigation etc.
- tooling during development right when needs it, not only later when trying to run it
- no more unit test for every line of code, can sleep again well at night
- knows all the stuff the programmer is otherwise supposed to know by heart
- statement completion in context, only the semantically valid, not just all variables the scope has access to like in other IDEs



## Zero-cost

- removes types completely on transpilation to JS
- no performance overhead since it all "transpiles away", as if you had written JS in the first place (but without bugs)
- no extra code is injected, transpiles away to plain JS
- just for error checking during development
- exact same runtime behavior and syntax, no translation, only layer on top
- type checking makes sense only during development anyways, on client during execution can not do anything, like a ship out on the sea, can not fix if it breaks down in production
- TS doesn't prevent transpilation if there is a type related error, since is still valid JS
- beware if has API that others are going to use, security is gone as soon as transpiled to JS, e.g. `readonly` properties, `private` class members ⚠️



## Superset

- technically it's own language, but follows exactly ECMAScript specs, doesn't diverge from standard, just adds static types
- (diverged from ECMAScript standard with enums, namespaces, decorators)
- like JavaScript with type annotations, no overhead in translating JS implementation into TS implementation
- implements new JS features when they reach Stage 3, can use newest features
- transpiles backwards for older ES versions, but better use only for types and use Babel for transpilation