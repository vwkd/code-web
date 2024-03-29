---
title: Type constructors
author: vwkd
index: 6
tags:
  - languages
  - typescript
---

- create custom types out of basic types



## Union types

- intersect multiple types
- values may be of any one of those types

```typescript
function greet(id: number | string) {
  console.log("Hello, agent " + id);
}

greet(7);
greet("Bond");
```

- only the _intersection_ of all the types' properties are available, e.g. `slice()` for type `<type>[] | string` but not `toUpperCase()`

```typescript
interface Bird {
  name: string;
  wingspan: number;
}

interface Fish {
  name: string;
  fins: number;
}

declare function getPet(): Bird | Fish

let pet = getPet();
pet.name;
pet.wingspan; // Property 'wingspan' does not exist on type 'Fish'.(2339)
```



## Intersection types

- combine multiple types
- values must be of all of those types

```typescript
function sayAge(person: {name: string} & {age: number}) {
  console.log(`${person.name} is ${person.age} years old.`);
}

sayAge({name: "Peter", age: 42});
sayAge({name: "Peter"}); // Property 'age' is missing in type '{ name: string; }' but required in type '{ age: number; }'.(2345)
```

- the union of all the types' properties are available, e.g. `toUpperCase()` and `push()` for type `<type>[] & string`
- for basic types the intersection is always empty since they are disjoint sets of values, needs to use with custom types, i.e. `number & string` is always `never`
- can use to extend existing types
- beware: if existing type is one of the basic types needs to use type assertion on declaration to prevent the more narrow basic type to be inferred, e.g. array ❗️

```typescript
type NamedArray = number[] & {name: string};

const arr = [1, 2, 3] as NamedArray;
arr.name = "Peter";

const brr = [4, 5, 6];
brr.name = "Lisa"; // Property 'name' does not exist on type 'number[]'.(2339)
```

- beware: intersection of two object types includes everything, e.g. call signatures, index signatures, etc. Use spread syntax to use only non-method properties ❗️


## Optional types

- union of type with `undefined`, e.g. `<type> | undefined`
- used to make variable or property optional
- append `?` after variable or property name
- e.g. in function argument, type alias, interface