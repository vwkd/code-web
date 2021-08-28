---
title: Type operators
author: vwkd
index: 8
tags:
  - languages
  - typescript
---

- type guard: operator that can narrow down types, e.g. `in`, `typeof`, `instanceof`, comparison operators, truthiness, etc.  
  beware: TS doesn't yet handle coercion, e.g. `==` might fail [#37251](https://github.com/microsoft/TypeScript/issues/37251)

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

if ("wingspan" in pet) {
  pet.wingspan;
} else {
  pet.fins;
}
```

```typescript
function typeSwitch(x: string | number): string {
  if (typeof x == "string") {
    return x.toUpperCase();
  } else if (typeof x == "number") {
    return x.toFixed(2);
  }
  throw new Error("Invalid input"); // needed because TS return analysis doesn't currently factor in complete control flow analysis
}
```

- use comparison operators to narrow down `null` or `undefined`

```typescript
function stringOnly(str: string | null): string {
  return str || "default";
}
```

- beware: doesn't work in more complex cases like nested functions, use non-null assertion operator `!`

```typescript
function stringOnly(str: string | null): void {
  function nested() {
    return str.toUpperCase(); // Object is possibly 'null'.(2531)
  }
  str = str || "default";
  nested();
}

function stringOnly(str: string | null): void {
  function nested() {
    return str!.toUpperCase();
  }
  str = str || "default";
  nested();
}
```



## `typeof` type operator

- in expression context gives JS type as string
- in type context gives TS type

```typescript
let x = "Hello World!";
let y: typeof x; // string
```



## Indexed type access operator

- get type of property in interface or object type
- use same bracket notation from object property accessor
- index is a literal string type, a union is distributed, i.e. `T[A | B]` is equivalent to `T[A] | T[B]`

```typescript
type Person = {
    name: string;
    age: number;
}

interface Person {
    name: string;
    age: number;
}

type Name = Person["name"]; // string
```



## Index type query operator

- get name of property in interface or object type
- name is itself a literal string type or union thereof
- can then use name as index for indexed type access operator

```typescript
type Person = {
    name: string;
    age: number;
}

type PersonKey = keyof Person; // "name" | "age"
type PersonVal = Person[PersonKey] // string | number
```

- can use with generic types

```typescript
// standalone version of property accessor
function getProperty<T, K extends keyof T>(object: T, key: K): T[K] {
  return object[key];
}

const o = { name: "Peter", age: 42 };

const name = getProperty(o, "name"); // "Peter"
```

```typescript
function getProperties<T, K extends keyof T>(object: T, keys: K[]): T[K][] {
  return keys.map(key => object[key]);
}

const o = { name: "Peter", age: 42 };

const name = getProperties(o, ["name", "age"]); // ["Peter", 42]
```

- for object type with numeric index signature returns basic type `number`, for string index signature `string | number` since since JS converts numeric indices to strings



## Mapped types

- transform object type
- can modify property types, add new ones, remove existing ones, etc.
- looks like object type with index signature, but is _own_ type, i.e. can _not_ add more properties, need to use intersection type ❗️

```typescript
type Keys = "name" | "age";
type Form = {
  [K in Keys]: boolean;
};

// equivalent to
type Form = {
  name: boolean;
  age: boolean;
}
```

- if keys are of type string or number the corresponding index signature is used

```typescript
type BooleanArray = {
  [K in number]: boolean;
};

const a: BooleanArray = [true, false];
```

- can use with generic, index type query and indexed type access operator to create powerful transformations, see Built-in generic types for many useful ones

```typescript
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

type Person = {
  name: string,
  age: number
};

type NullablePerson = Nullable<Person>;
                //  = {
                //      name: string | null;
                //      age: number | null;
                //    }
```

```typescript
type Nested<S> = {
  value: S;
  modified: boolean;
  printValue(): void;
};

type Wrapper<T> = {
  [P in keyof T]: Nested<T[P]>;
};

type Person = {
  name: string,
  age: number
};

type WrappedPerson = Wrapper<Person>;
                // = {
                //      name: Nested<string>;
                //      age: Nested<number>;
                //   }

const p: WrappedPerson = {
  name: {
    value: "Peter",
    modified: false,
    printValue() {
      console.log(`My name is ${this.value}.`);
    }
  },
  age: {
    value: 42,
    modified: false,
    printValue() {
      console.log(`I am ${this.value} years old.`);
    }
  },
}

p.name.printValue(); // My name is Peter.
p.age.printValue(); // I am 42 years old.
```



## Conditional types

- choose type based on condition
- can replace overloads with conditional type
- syntax of ternary operator in JS

```typescript
T extends U ? X : Y
```

- can't be resolved if condition depends on type variables that can't be evaluated yet, is deferred, until then is carried through as is

```typescript
// resolved
type Return<T> = T extends string ? number : string;

declare function typeSwitch<T>(x: T): Return<T>;

const x = typeSwitch("Peter"); // number
const y = typeSwitch(42); // string
```

```typescript
// deferred
type Return<T> = T extends string ? number : string;

declare function typeSwitch<T>(x: T): Return<T>;

function func<S>(x: S): void {
  const y = typeSwitch(x); // Return<S>
}
```

- beware: type variables don't get type narrowing, because are only determined at call site, needs to use type assertions, see [#22735](https://github.com/microsoft/TypeScript/issues/22735#issuecomment-376960435), [#24929](https://github.com/microsoft/TypeScript/issues/24929), [StackOverflow](https://stackoverflow.com/questions/60475431/generic-type-extending-union-is-not-narrowed-by-type-guard)
```typescript
type Return<T> = T extends string ? number : T extends number ? string : never;

function typeSwitch<T extends string | number>(x: T): Return<T>{
  if (typeof x == "string") {
    return 42; // Type '42' is not assignable to type 'Return<T>'.(2322)
  } else if (typeof x == "number") {
    return "Hello World!"; // Type '"Hello World!"' is not assignable to type 'Return<T>'.(2322)
  }
  throw new Error("Invalid input"); // needed because TS return analysis doesn't currently factor in complete control flow analysis
}

const x = typeSwitch("qwerty"); // string | number instead of just number
```

- conditional types distribute over union types, i.e. `A | B extends U ? X : Y` is equivalent to `(A extends U ? X : Y) | (B extends U ? X : Y)`
- can use distributed conditional types and `never` to filter types

```typescript
type A = Exclude<"a" | "b", "a" | "c">; // "b"
type B = Extract<"a" | "b", "a" | "c">; // "a"
type C = NonNullable<"a" | null | undefined>; // "a"
```

- can use with mapped types to filter object type

```typescript
interface Person {
  name: string;
  age: number;
  sayHi(): void;
}

type FunctionPropertyNames<T> = {
  [K in keyof T]: T[K] extends Function ? K : never;
}[keyof T];

type FunctionProperties<T> = Pick<T, FunctionPropertyNames<T>>;

type A = FunctionPropertyNames<Person>; // "sayHi"
type B = FunctionProperties<Person>; // { sayHi: () => void; }
```

- can `infer` a type variable within the `extends` clause, don't need to declare the inferred type variable before the `extends`

```typescript
// infered
type Flatten<T> = T extends (infer U)[] ? U : T;

// manually using indexed type access operator
type Flatten<T> = T extends any[] ? T[number] : T

type x = Flatten<string[]>; // string
type y = Flatten<number>; // number
```

- multiple types are inferred as a union type or intersection type depending on the context

```typescript
type Props<T> = T extends { a: infer U; b: infer U } ? U : never;
type A = Props<{ a: string; b: string }>; // string
type B = Props<{ a: string; b: number }>; // string | number
```

```typescript
type Args<T> = T extends { a: (x: infer U) => void; b: (x: infer U) => void } ? U : never;
type A = Args<{ a: (x: string) => void; b: (x: string) => void }>; // string
type B = Args<{ a: (x: string) => void; b: (x: number) => void }>; // string & number (= never)
```

- when inferring from a overloaded function, only the last call signature is used ❗️