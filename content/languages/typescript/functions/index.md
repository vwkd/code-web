---
title: Functions
author: vwkd
index: 3
tags:
  - languages
  - typescript
---

```typescript
function add(x: number, y: number): number {
    return x + y;
}
```

- argument types are specified like for a variable, return type after argument list
- don't need to specify types of return statement since are inferred, but better to fix it now, or could later introduce a mistake when modifying the function pp
- by default, TS requires that number of arguments passed matches number of parameters, i.e. no forgotten / additional arguments ❗️
- can make parameters optional using `?` sign after name, required parameters must be ordered before any optional ones, or alternatively use default parameters, results in the same optional type
- rest parameters must be of type array, i.e. `<type>[]` or `Array<type>`

```typescript
function sum(...args: number[]): number {
    return args.reduce((accumulator, currentValue) => accumulator + currentValue);
}

sum(1, 2, 3, 4, 5, 6, 7); // 28
```

- can specify a special first parameter `this` with type of `this`, by default is `any`, completely erased in JS, e.g. limit method calls to class instances it is part of, but consider using arrow functions that don't have their own `this` anyways
- can set `this` to `void` to prevent function from using it, e.g. in function parameters expecting callbacks

```typescript
function multiply(arr: number[], func: (this: void, item: number) => number): number[] {
    return arr.map(func);
}

multiply([1, 2, 3], n => n * 2);
multiply([1, 2, 3], function (n) { return this.foo; }); // Property 'foo' does not exist on type 'void'.ts(2339)
```



## Overloads

- specify multiple function types for the same function
- used if function returns different output types based on different input types

```typescript
function typeSwitch(x: string | number): string | number {
  if (typeof x == "string") {
    return 42;
  } else if (typeof x == "number") {
    return "Hello World!";
  }
}

const x = typeSwitch("qwerty"); // string | number instead of just number 👎
```

- can specify arbitrarily many overloads before function definition
- actual function definition must satisfy all overloads
- does not mean there are multiple declarations of a function, just looks like it from a statical type perspective

```typescript
function typeSwitch(x: string): number;
function typeSwitch(x: number): string;
function typeSwitch(x: string | number): string | number {
  if (typeof x == "string") {
    return 42;
  } else if (typeof x == "number") {
    return "Hello World!";
  }
  throw new Error("Invalid input"); // needed because TS return analysis doesn't currently factor in complete control flow analysis
}

const x = typeSwitch("qwerty"); // number 👍
```

- arguments in function call must match at least one overload
- compiler accepts first matching overload even if another one would also match, i.e. sort overloads from most specific to least specific

```typescript
function sayHi(this: Person): void {
  console.log("Hi, my name is " + this.name);
}

class Person {
  public name: string;
  public greet: () => void;
  constructor(name: string) {
    this.name = name;
    this.greet = sayHi;
}
}

const p = new Person("Peter");

console.log(p.greet());
console.log(sayHi()); // The 'this' context of type 'void' is not assignable to method's 'this' of type 'Person'.(2684)
```

```javascript
function sayHi() {
    console.log("Hi, my name is " + this.name);
}

class Person {
    constructor(name) {
        this.name = name;
        this.greet = sayHi;
    }
}
const p = new Person("Peter");
console.log(p.greet()); // Hi, my name is Peter
console.log(sayHi()); // Hi, my name is 
```



## Signatures

- use call signature in object type for callable function with properties
- syntax uses colon instead of arrow between parameter list and return type ❗️

```typescript
type Greeter = {
    location: string;
    (): string;
}

function print(func: Greeter) {
    console.log(func() + func.location);
}

function greeter(): string {
    return `Hi, welcome to `;
}
greeter.location = "Disney Land";
print(greeter);
```

- for constructor function needs to add `new` in front of call signature

```typescript
// Date constructor can be called with or without new
type CallOrConstruct = {
    new(s: string): Date;
    (n?: number): number;
}
```