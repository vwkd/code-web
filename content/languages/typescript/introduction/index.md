---
title: Introduction
author: vwkd
index: 1
tags:
  - languages
  - typescript
---

## Implicit typing

- types get inferred where ever possible, e.g. in variable assignment, return statement of function, object properties, array elements, etc.
- don't need to provide types for everything, but safer so can not later accidentally change them, and use them for documentation

```typescript
let age = 42;

age.toUpperCase(); // Property 'toUpperCase' does not exist on type 'number'.ts(2339)
```

- the declared type determines the types that are allowed to be assigned, even if in meantime had a more specific type

```typescript
let x = Math.random() < 0.5 ? 42 : "world"; // string | number

x = 1;
x = "goodbye!";
// both valid assignments since declared type was string | number
```

- variable declarations with `const` infer literal type, i.e. `const x = 42` is inferred to be of type `42` not of type `number` ❗️
- variable declarations with `let` and `var` infer general type, i.e. `let x = 42` is inferred to be of type `number` not of type `42` ❗️

```typescript
let x = 42; // number
let y: 42 = 42; // 42

x = 21;
y = 21; // Type '21' is not assignable to type '42'.(2322)
```

- `readonly` properties infer the literal type, similarly to `const` variable declarations 



## Structural typing

- type compatibility is checked using duck typing ("If it walks like a duck and quacks like a duck, it is a duck!")
- an object type is compatible with another, as long as it implements _at least_ all properties of the other

```typescript
interface Named {
  name: string;
}

function sayHi(obj: Named) {
  console.log(`Hi, my name is ${obj.name}.`);
}

// 'p' implements at least all properties of 'Named'
const p = { name: "Peter", age: 42 };
sayHi(p);
```

```typescript
interface Named {
  name: string;
}

class Person {
  name: string;
  age: number;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

// 'Person' implements at least all properties of 'Named'
const q: Named = new Person("Peter", 42);
```

- excess property checking: object literals aren't allowed to have more properties than specified when assigning them directly to variables or passing them as arguments to functions, often source of bugs, can allow by assigning it to new variable with type inference and using that variable

```typescript
interface Named {
  name: string;
}

function sayHi(obj: Named) {
  console.log(`Hi, my name is ${obj.name}.`);
}

sayHi({ name: "Peter", age: 42 }); // Object literal may only specify known properties, and 'age' does not exist in type 'Named'.(2345)

const p = { name: "Peter", age: 42 };
sayHi(p);
```

```typescript
interface Named {
  name: string;
}

const q: Named = { name: "Peter", age: 42 }; // Object literal may only specify known properties, and 'age' does not exist in type 'Named'.(2345)

const p = { name: "Peter", age: 42 };
const q: Named = p;
```

- if all properties are optional, must at least implement one, i.e. not none

```typescript
interface Named {
  gender?: string;
}

const p = { name: "Peter", age: 42 };
const q: Named = p; // Type '{ name: string; age: number; }' has no properties in common with type 'Named'.(2559)
```

- a function type is compatible with another, as long as the other implements _at least_ all parameter types of it and the other type's return type is a subtype of its return type, the parameter names can be different

```typescript
let f = (a: number) => ({name: "Peter", age: 42});
let g = (x: number, y: string) => ({name: "Peter"});

g = f; // ✅
f = g; // ❌
```

- function parameter bivariance: parameter types of function types are compatible, as long as one is assignable to the other, using `strictFunctionTypes` flag only if parameter type of source is assignable to parameter type of target

```typescript
interface Named {
    name: string;
}

interface Person {
    name: string;
    age: number;
}

let f = (a: Named) => {};
let g = (b: Person) => {};

g = f; // ✅, parameter of source is subtype of parameter of target
f = g; // ✅, parameter of target is subtype of parameter of source, ❌ with strictFunctionTypes flag
```

- classes are compatible, if their instance object types are compatible, i.e. static properties and the constructor are ignored ❗️
- beware: classes containing private or protected instance properties are _not_ compatible, only if those instance properties originate from the same class



## Control Flow Analysis

- TS narrows down types to more specific types when it can
- in if statements, switch statements, loops, conditional ternaries, etc.
- using type guards in condition, e.g. `typeof`, `instanceof`, equality operator, truthiness, see Type operators
- beware: TS doesn't catch all edge cases of JS, needs to still understand JS ❗️
- can check if type is `never` to not forget a code path, e.g. a missing case in switch statement, if...else statements, etc.

```typescript
function assertNever(object: never): never {
    throw new Error("You forgot to handle a control flow path.")
}

type Shape = { kind: "circle" } | { kind: "rectangle" } | { kind: "square" };

declare let shape: Shape;

switch (shape.kind) {
    case "circle":
        /* ... */
        break;
    case "rectangle":
        /* ... */
        break;
    default:
        assertNever(shape); // Argument of type '{ kind: "square"; }' is not assignable to parameter of type 'never'.(2345)
}
```



## Resources

- [The New TypeScript Handbook](https://github.com/microsoft/TypeScript-New-Handbook)
- [Anders Hejlsberg - What's new in TypeScript](https://channel9.msdn.com/Events/Speakers/Anders-Hejlsberg)
- [TSConf 2018](https://www.youtube.com/playlist?list=PL2z7rCjEG2ksF0rJ8Qwp1y5eTjqiPIRfT), [TSConf 2019](https://www.youtube.com/playlist?list=PL2z7rCjEG2kumYQw0tl-afLYWUk-kKREB)
- [dotJS 2018 - Anders Hejlsberg - TypeScript: Static types for JavaScript](https://www.youtube.com/watch?v=ET4kT88JRXs)