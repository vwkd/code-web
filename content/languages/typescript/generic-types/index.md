---
title: Generic types
author: vwkd
index: 7
tags:
  - languages
  - typescript
---

- type variables that are used in place of specific types
- used to specify relationship between types without needing to know actual types
- actual types are specified later in call, either inferred or explicitly
- type variables / parameters are in angle brackets after name of function, class, type alias, interface, etc.
- multiple type variables / parameters can be separated by commas
- by convention, type variable names are single uppercase letters, e.g. `T`
- use instead of `any` type to make expression again type safe
- can't do operations that are not valid for any type, needs to first narrow type using type guards, also check if is non-nullable ❗️

```typescript
// function with generic
function echo<T>(input: T): T {
  return input;
}

const a = echo<number>(42); // number, explicit type parameter
const b = echo(42); // number, inferred type parameter
const c = echo<string>("Hello World"); // string, explicit type parameter
const d = echo("Hello World"); // string, inferred type parameter

// function type with generic
const identity: <U>(input: U) => U = echo;
```

```typescript
// class with generic
class Person<T> {
  constructor(public prop: T) {}
}

const a = new Person("Peter");
a.prop; // string
const b = new Person(42);
b.prop; // number
```

```typescript
// type aliase with generic
type Person<T> = {
  x: T,
  y: T,
};

const a: Person<number> = {x: 42, y: 21};
const b: Person<string> = {x: "foo", y: 21}; // Type 'number' is not assignable to type 'string'.(2322)
```

```typescript
// interface with generic
interface Person<T> {
  x: T,
  y: T,
}

const a: Person<boolean> = {x: true, y: false};
const b: Person<string> = {x: 42, y: "bar"}; // Type 'number' is not assignable to type 'string'.(2322)
```

```typescript
// standalone version of Array.prototype.map()
function map<T, R>(arr: T[], func: (item: T) => R): R[] {
  return arr.map(func);
}

map(["Hello", "World"], n => n * 2);
// TS2362: The left-hand side of an arithmetic operation must be of type 'any', 'number', 'bigint' or an enum type.
```

- can specify default type for type parameters, required type parameters must come first
- in function or class a call can often omit type parameters since types are inferred from arguments, not in type alias or interface though
- always use as few type variables as possible to avoid complexity ❗️
- don't use generics if basic types can be used, i.e. if type variable only appears in one location ❗️
<!-- todo: delete above?? works for simple types, not if wants to spread it, etc. ?!?! -->
- can't use generic type parameter with static properties of a class ❗️ 
- currently does not support control-flow type narrowing on type variables ([StackOverflow](https://stackoverflow.com/questions/60475431/type-is-not-assignable-to-conditional-type-for-generic))

```typescript
type Return<T> = T extends string ? number : T extends number ? string : never;

function typeSwitch<T extends string | number>(x: T):  Return<T>{
  if (typeof x == "string") {
    return 42; // Type '42' is not assignable to type 'Return<T>'.(2322)
  } else if (typeof x == "number") {
    return "Hello World!"; // Type '"Hello World!"' is not assignable to type 'Return<T>'.(2322)
  }
  throw new Error("Invalid input"); // needed because TS return analysis doesn't currently factor in complete control flow analysis
}

const x = typeSwitch("qwerty"); // number
```

- recursion cut-off level when comparing two identical recursive types is 5

```typescript
type Foo<T> = {
  item: T
  next: Foo<Foo<T>>;
}

type Bar<T> = {
  item: T
  next: Bar<Bar<T>>;
}
```

- members of a union type are distributed over generic

```typescript
type Named = { name: string };

type filterNamed<T> = T extends Named ? T : never;

type Person = {
    name: string;
}

type Animal = {
    name: string;
}

type Car = {
    speed: number;
}

type NamedTypes = filterNamed<Person | Animal | Car>;
             // = filterNamed<Person> | filterNamed<Animal> | filterNamed<Car>;
             // = Person | Animal | never
             // = Person | Animal
```

- as array can spread elements in tuple

```typescript
function tail<T extends any[]>(arr: [any, ...T]) {
	const [_, ...rest] = arr;
	return rest;
}
```

```typescript
// standalone version of Array.prototype.concat()
function concat<T extends any[], U extends any[]>(arr1: T, arr2: U) {
	return [...arr1, ...arr2];
}
```



## Constraints

- limit a generic to certain subset of types
- use the `extends` keyword after type variable
- `extends` means left type is assignable to right type, types are compatible, not necessarily equal ❗️
- keep it as simple as possible, do not over-complicate constraints ❗️

```typescript
// could have as well used a separate type or interface for { length: number; }.
function getLength<T extends { length: number }>(arg: T): number {
  return arg.length;
}

getLength([1, 2, 3]);
getLength("alice");
getLength(1000); // Argument of type '1000' is not assignable to parameter of type '{ length: number; }'.(2345)
```

```typescript
interface Named {
  name: string;
}

function sortByName<T extends Named>(nameArray: T[]): T[] {
  nameArray.forEach(item => { console.log(item.name) }); // can access properties of Named
  return nameArray;
}

const arr = [
  { name: "Peter", age: 42, alive: true },
  { name: "Susan", age: 99, alive: false },
  { name: "Fox", age: 21, alive: true }
]; // compatible with type Named[]

const sortedArr = sortByName(arr); // { name: string; age: number; alive: boolean; }[]
```

- can constrain type parameter by another type parameter

```typescript
function getProperty<O, K extends keyof O>(obj: O, key: K): O[K] {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a");
getProperty(x, "f"); // Argument of type '"f"' is not assignable to parameter of type '"a" | "b" | "c" | "d"'.(2345)
```



## Inferred type

- ???

```typescript
type ReturnTypeOrType<T> = T extends (...args: any[]) => infer R ? R : T;

function sum(...args: number[]): number {
    return args.reduce((acc, cur) => acc + cur, 0);
}

type a = ReturnTypeOrType<typeof sum>; // number
type b = ReturnTypeOrType<{ name: string }>; // { name: string }
```

- element type

```typescript
type ElementType<T extends Iterable<any>> = T extends Iterable<infer E> ? E : never;
```



## Built-in generic types

- `Partial<T>`: Make all properties in T optional
- `Required<T>`: Make all properties in T required
- `Readonly<T>`: Make all properties in T readonly
- `Pick<T, K>`: From T, pick a set of properties whose keys are in the union K
- `Record<K, T>`: Construct a type with a set of properties K of type T
- `Exclude<T, U>`: Exclude from T those types that are assignable to U
- `Extract<T, U>`: Extract from T those types that are assignable to U
- `Omit<T, K>`: Construct a type with the properties of T except for those in type K
- `NonNullable<T>`: Exclude null and undefined from T
- `Parameters<T>`: Obtain the parameters of a function type in a tuple
- `ConstructorParameters<T>`: Obtain the parameters of a constructor function type in a tuple
- `ReturnType<T>`: Obtain the return type of a function type, argument is function type not function itself, i.e. `typeof f` not just `f`
- `InstanceType<T>`: Obtain the return type of a constructor function type
- `ThisType<T>`: Marker for contextual 'this' type
- `ThisParameterType<T>`: Extracts the type of the 'this' parameter of a function type, needs `strictFunctionTypes` flag
- `OmitThisParameter<T>`: Removes the 'this' parameter from a function type, needs `strictFunctionTypes` flag