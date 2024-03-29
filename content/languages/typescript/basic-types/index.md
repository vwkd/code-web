---
title: Basic Types
author: vwkd
index: 2
tags:
  - languages
  - typescript
---

- type: set of values, set theory
- type system: rules that assign types to entities of programming language
- TypeScript has a more restricted type system than JavaScript
- unit types: every value is a type, e.g. there is a type "`42`" with only possible value `42`
- basic types: common sets of unit types, e.g. `number`, `string`
- can assign type to variables, function parameters, function return type, object properties, etc.
- uses colon after entity name, like in classical languages

```typescript
const age: number = 42;

function sayAge(age: number): string {
    return `Hello, I am ${age} years old.`;
}
```

- type gets fixed once set, can't assign other type later like in JS, otherwise typing wouldn't make any sense since couldn't be used to reason about code



## Basic Types

| types | description |
| ----- | ----------- |
| `string` | string values (* and `null` and `undefined`) |
| `number` | number values (* and `null` and `undefined`) |
| `bigint` | bigint values (* and `null` and `undefined`) |
| `boolean` | boolean values (* and `null` and `undefined`) |
| `symbol` | symbol values (* and `null` and `undefined`) |
| `object` | object values (* and `null` and `undefined`) |
| `function` | symbol values (* and `null` and `undefined`) |
| `undefined` | `undefined` (* and `null`) |
| `null` | `null` (* and `undefined`) |
| `any` | any value, no type checking, "unsafe" |
| `unknown` | any value, restrictive type checking, "safe" |
| `never`| non-existent value, subtype of every other type, e.g. if function throws error, never returns, or if-testing covered every possible type |
| `void` | no value, subtype of `undefined` (* and `null`), e.g. function that implicitly returns undefined |
| `[<type>, ..., <type>]` | tuple of different types, known length and order, items of different types |
| `<type>[]` | array of `<type>`, variable length and order, items of same type |
| `{key1: <type>, ..., keyN: <type>}` | object type, known length |
| `(arg1: <type>, ..., argN: <type>) => <type>` | function type |

- (*) by default `null` and `undefined` are subtypes of every other type, e.g. can assign `undefined` to `number`, disable using `strictNullChecks` flag 
- ??? always use literal types (lowercase), constructor ones are special built-in types (uppercase), e.g. `object` not `Object` ???
- ??? can also use constructor names instead, e.g. `Array<type>`, but not often used ???
- types need to be written after object or array destructuring

```typescript
// object
const o = {a: "foo", b: 12, c: "bar"};
const { a, b }: { a: string, b: number } = o;

// tuple
const [name, age]: [string, number] = ["Peter", 42];
```

- can make arrays and tuples readonly using `readonly` before name, disallows mutating its properties, effectively like const for object properties
- can make objects readonly by using a type alias / interface with `Readonly<T>` utility type, see [Type constructors](#)
- template string type with back-ticks constructs string-like type by concatenating or matching pattern of other string-like types



## Object Type

- types of object properties, shape of object
- uses object literal syntax, e.g. `{ a: string, b: number }`
- property separator can be comma or colon ❗️

```typescript
const o: { a: string, b: number } = { a: "foo", b: 12 };
```

- used as type where ever an object is expected, e.g. when defining the arguments of a function
- don't confuse with object definition itself, doesn't need types since can be inferred from values
- type inference in variable assignment of object literal is close-ended, can not add new properties ❗️

```typescript
const o = { a: "foo", b: 12 };
o.c = 42; // Property 'c' does not exist on type '{ a: string; b: number; }'.ts(23399)
```

<!-- TODO: comment below is lost ? -->
 variables object literal that initializes a variable declaration gives its type to the declaration
- can make object properties readonly using `readonly` before property name, effectively like `const` for object properties

```typescript
const o : { readonly a: string, b: number } = { a: "foo", b: 12 };

o.b = 14;
o.a = "bar"; // TS2540: Cannot assign to 'a' because it is a read-only property.
```

- can make properties optional using `?` sign after name
- a getter without a setter is automatically inferred to be `readonly`
- the getter type must be a subtype of the setter type



## Function Type

- type of function, shape of a function
- uses arrow function syntax, e.g. `(a: number, b: number) => number`

```typescript
function map(items: any[], mappingFunction: (item: any) => any): any[] {
  /* ... */
}
```

- used as type where ever a function is expected, e.g. when defining the arguments of a function
- don't confuse with function definition itself (see 4. Functions), especially since syntax is equal to arrow function definition
- parameter names in function type must not match the parameter names of the function, as long as the order and the types match ❗️
- can make parameters optional using `?` sign after name



## Tuple Type

- like array but with known length and order
- can make elements optional using `?` sign after name
- can spread tuple elements

```typescript
type A = [string, string]
type B = [number, number]
type C = [...A, ...B]
```

- can spread single array element, expands tuple to arbitrary length
- beware: no optional types can come after an array element

```typescript
type args = [string, ...number[], boolean];

const x: args = ["Hello", true];
const y: args = ["Hello", 42, true];
const z: args = ["Hello", 42, 21, 1, 2, 3, true];
```

- can name elements

```typescript
type args = [first: string, second: boolean, ...rest: number[]]
```



## Literal Types

- values of type boolean, string, number can themselves be used as types
- literal types are subtypes of their respective general types, i.e. `42` is a subtype of `number`
- together with union types can use to restrict input to certain values, like enums

```typescript
function sayDirection(dir: 'up' | 'left' | 'down' | 'right'): void {
    console.log(`Please go ${dir}.`)
}
sayDirection('up');
sayDirection('forward'); // Argument of type '"forward"' is not assignable to parameter of type '"up" | "left" | "down" | "right"'.(2345)
```

- discriminated union: union of object types which contain a common property, at least one must be of a literal type and for all it must not be a generic type variable

```typescript
type Shape = { kind: "square" } | { kind: null } | { kind: string };
```

- discriminated union is narrowed based on the common property, e.g. in if statement, switch statement, etc.

```typescript
type Shape = { kind: 42 } | { kind: null } | { kind: string };

function foo(shape: Shape): Shape["kind"] {
  if (shape.kind) {
    // string | 42
    return shape.kind;
  } else {
    // string | null
    return shape.kind;
  }
}
```

- can use discriminated union and functional stype to replace entire class hierarchies in class style 

```typescript
// class-oriented style
abstract class Shape {
    abstract getArea(): number;
};

class Circle extends Shape {
    constructor(public radius: number) {
        super();
    }
    getArea() {
        return Math.PI * this.radius ** 2;
    }
}

class Rectangle extends Shape {
    constructor(public w: number, public h: number) {
        super();
    }
    getArea() {
        return this.w * this.h;
    }
}

class Square extends Shape {
    constructor(public size: number) {
        super();
    }
    getArea() {
        return this.size ** 2;
    }
}

const myshape: Shape = new Circle(10);
const myarea = myshape.getArea();
```

```typescript
// functional style
function assertNever(object: never): never {
    throw new Error("Invalid object.")
}

// discriminated union, the tag is the 'kind' property
// could have also put them into separate interfaces
type Shape =
    { kind: 'circle', radius: number } |
    { kind: 'rectangle', height: number, width: number } |
    { kind: 'square', size: number }

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "rectangle":
            return shape.height * shape.width;
        case "square":
            return shape.size ** 2;
    }
    assertNever(shape);
}

const myshape: Shape = { kind: "circle", radius: 10 };
const myarea = getArea(myshape);
```



## Enum Type

- (beware: diverging feature from JavaScript, use literal types instead! ⚠️)
- fixed length set of named constants
- used to create limited set of distinct cases, e.g. boolean type is an enum, can only select between `true` and `false`
- accesses constants like object properties
- like freezed object, where values don't matter except that they are distinct

```typescript
enum Direction {
  Up,
  Right,
  Down,
  Left
}

function walk(dir: Direction) {
  switch (dir) {
    case Direction.Up:
      return "Walking up...";
    case Direction.Right:
      return "Walking right...";
    case Direction.Down:
      return "Walking down...";
    case Direction.Left:
      return "Walking left...";
    default:
      throw new Error("Invalid input.")
  }
}

console.log(walk(Direction.Up)); // Walking up...
console.log(walk(Direction.Forward)); // Property 'Forward' does not exist on type 'typeof Direction'.(2339)
```

- constants have distinct values so can be compared and assigned
- by default values are ascending non-negative integers
- can access name using value and value using name
- implemented as object that has two opposite key-value pairs for each constant, i.e. name-value and value-name

```typescript
enum Direction {
  Up,
  Right,
  Down,
  Left
}

const dir = Direction.Up; // Direction.Up

console.log(dir); // 0
console.log(Direction[dir]); // Up
```

```javascript
// JS implementation
Direction = {
  0: "Up",
  1: "Right",
  2: "Down",
  3: "Left",
  Up: 0,
  Right: 1,
  Down: 2,
  Left: 3
}
```

- can specify different numbers as values, later values will ascend from that

```typescript
enum Direction {
  Up = 100,
  Right,
  Down,
  Left
}

const dir = Direction.Right; // Direction.Right

console.log(dir); // 101
console.log(Direction[dir]); // Right
```

- can specify strings as values, but looses ability to access name using value ⚠️

```typescript
enum Direction {
  Up = "u",
  Right = "r",
  Down = "d",
  Left = "l"
}

const dir = Direction.Right; // Direction.Right

console.log(dir); // r
console.log(Direction[dir]); // Up
```

```javascript
// JS implementation
Direction = {
  Up: "u",
  Right: "r",
  Down: "d",
  Left: "l"
}
```

- can compute values dynamically, any non-initialised member needs to come first since otherwise it doesn't know from where to increment
- can make enums `const`, allow only constant enum expressions ???, are removed completely on compile time and replaced by values



## Type assertion

- treats entity as a more or as a less specific version of same type
- often used when TS is too broad when inferring types, e.g. when selecting DOM nodes
- add type assertion either by post-pending entity with `as <type>` or pre-fixing it with `<type>`

```typescript
const myCanvas = document.getElementById("main_canvas"); // HTMLElement | null

const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement; // HTMLCanvasElement

const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas"); // HTMLCanvasElement
```

- be careful since can even change type completely ❗️

```typescript
const name = ("Peter" as any) as number; // number
```

### const assertion

- assert that type is immutable, takes narrowest possible type
- use `as const` after literal value
- for primitive literal value, i.e. string, number, etc., narrows type down to literal type
- for non-primitive literal value, i.e. array, object, etc., narrows type down to read only, like `readonly` before
- used because object and array types are taken to be mutable by default, i.e. types of object properties and array items are inferred as general types instead of literal types since values could be changed at any time

```typescript
const a = {
  kind: "circle",
  radius: 10
};

/*
{
  kind: string;
  radius: number;
}
*/

const b = {
  kind: "circle",
  radius: 10
} as const;

/*
{
  readonly kind: "circle";
  readonly radius: 10;
}
*/

const x = [1, 2, "world"]; // (string | number)[]
const y = [1, 2, "world"] as const; // readonly [1, 2, "world"];
const y: readonly [1, 2, "world"] = [1, 2, "world"];
```

- use `as <value>` after property value / array item to assert for a single property / array item

```typescript
function sayDirection(dir: 'up' | 'left' | 'down' | 'right'): void {
    console.log(`Please go ${dir}.`)
}

const myobj = {mydir: 'up'}; // property 'mydir' is of type 'string', not of type 'up'
sayDirection(myobj.mydir); // TS2345: Argument of type 'string' is not assignable to parameter of type '"up" | "left" | "down" | "right"'.
sayDirection(myobj.mydir as 'up'); // specify myobj.mydir as type 'up'

const myobj2 = {mydir: 'up' as 'up'}; // property 'mydir' is of type 'up'
sayDirection(myobj2.mydir);
```

### Non-null assertion operator

- asserts that value isn't `null` or `undefined`
- append `!` after variable
- use cautiously ⚠️
