---
title: Type aliases and interfaces
author: vwkd
index: 4
tags:
  - typescript
---

## Type Aliases

- alias for a type
- can be reused instead of typing out literal type
- alias names must be unique

```typescript
// variable type alias
type ID = number | string;

// object type alias
type Person = {
  firstName: string,
  lastName: string,
  age: number
};

// function type alias
type NumToString = (num: number) => string;
```

- aliases don't create new types, are just names for existing ones, i.e. can have two aliases for same type ❗️

```typescript
type s1 = string;
type s2 = string;

const p1: s1 = "Hello";
const p2: s2 = p1; // legal, since s1, s2 are aliasing the identical type string
```

- type aliases for object types can be extended using intersection types

```typescript
type Person = {
    name: string;
    age: number;
}

type Work = {
    profession: string;
}

type Adult = Person & Work;

const person1: Adult = {age: 42, name: "Peter", profession: "Teacher"};
```



## Interfaces

- alias for an _object_ type
- order of object properties doesn't matter
- use semicolons instead of colons, and no equal sign ❗️

```typescript
// object interface
interface Person {
  firstName: string;
  lastName: string;
  age: number;
  sayHi: (this: Person) => string; // bind this context to Person object
};
  
const person1: Person = {
  age: 42,
  firstName: "Peter",
  lastName: "Griffin",
  sayHi: function() {return "Hi, my name is" + this.firstName}
};

// function expression interface
interface NumToString {
  (num: number): string
}

const sayAge: NumToString = function(age: number) {
    return `Hello, I am ${age} years old.`;
}
```

- interfaces can extend other interfaces using the `extend` keyword, conciser syntax than intersection type with type aliases, multiple interfaces can be specified using comma

```typescript
interface Person {
    name: string;
    age: number;
}

interface Adult extends Person {
    profession: string;
}

const person1: Adult = {age: 42, name: "Peter", profession: "Teacher"};
```

- can even extend classes, class members are used as if they had been declared in interface without implementation
- interfaces allow for declaration merging, redeclaration of same interface, e.g. used for monkey-patching existing libraries

```typescript
interface Person {
    name: string;
    age: number;
}

interface Person {
    profession: string;
}

const person1: Person = {age: 42, name: "Peter", profession: "Teacher"};
```

- differences between interface and type alias for object type, see [StackOverflow](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/52682220#52682220)

    | point | interface | type alias |
    | ----- | --------- | ---------- |
    | extension | `extends`, declaration merging | intersection type |
    | error messages | better | - |

- for object types, interfaces are almost like type aliases, except they can be extended by redeclaration ("declaration merging") or the `extend` keyword instead of the intersection type, also they have clearer error messages, prefer interfaces over type aliases, see [StackOverflow](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/52682220#52682220)



## Index Signatures

- specify type of object properties according to how they're indexed
- can use to type arbitrarily many properties in object literal, type alias, interface, class, etc.
- can be string or number, because JS allows indices to be string or number, e.g. `obj["somestr"]` and `obj[42]`
- but number index signature must be subtype of string index signature, since JS converts numeric indices to strings, e.g. `obj[100]` is actually just `obj["100"]` ❗️

```typescript
interface Person {
    [index: string]: string;
}

const p: Person = { name: "Peter", surname: "Wikler", age: 42 }; // Type 'number' is not assignable to type 'string'.(2322)
```

```typescript
interface Person {
    [index: string]: string | boolean;
    [index: number]: boolean
}

const p: Person = { name: "Peter", employed: true, "42": false };
const a = p[42]; // boolean
const b = p["42"]; // string | boolean
```

- beware: property names that contain a number written as string are also treated according to the numeric index signature, i.e. can't assign it to a type only covered by the string index signature

```typescript
interface Person {
    [index: string]: string | boolean;
    [index: number]: boolean
}

const p: Person = { name: "Peter", employed: true, "42": "foobar" }; // Property '"42"' is incompatible with index signature.
const a = p[1]; // boolean
const b = p["1"]; // string | boolean
```

- any additional properties must be a subtype of the string index signature

```typescript
interface Person {
    [index: string]: string;
    name: string;
    age: number; // Property 'age' of type 'number' is not assignable to string index type 'string'.(2411)
}

const p: Person = { name: "Peter", age: 42, employer: "Ford" };
```

- beware: the indices covered by a string index signature and numeric index signature are _disjoint_ sets, numeric indices are not treated as subset of string indices, only the _type_ of the numeric indices must be a subtype of the type of string indices because `obj[100]` is `obj["100"]`, but the types of the indices themselves remain disjoint sets, i.e. an array can't have a string index signature but just a numeric index signature ❗️

```typescript
interface StringArray {
    [index: number]: boolean;
    [key: string]: boolean | string;
}

let myArray: StringArray = [true, false] as any; // needs any since string[] is not treated as subtype of object
myArray[2] = true;
myArray.something = "Hello World!";
```

- can make index signature readonly by prepending `readonly`
- an object type is compatible with a type with an index signature, as long as all of its property types are compatible with the index signature
