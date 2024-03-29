---
title: Basics
author: vwkd
index: 2
tags:
  - languages
  - javascript
---

<!-- todo: consider removing all console.log() statements, also in all subsequent files, because is Web API -->

## Syntax

- case sensitive
- uses Unicode charset, e.g. Umlaute are allowed
- semicolon after each statement, not mandatory since interpreter infers missing ones from line break, but leaving out may lead to unexpected errors line break was unintended
- whitespace is ignored, only for readability, space, tab, newline, comments get converted to whitespace on interpretation
- single-line comments `//`, multi-line comments `/*...*/`
- statements get interpreted in order of appearance
- beware: automatic semicolon insertion in parser, can hide pesky bugs, e.g. blank return statement, always use semicolons ⚠️



## Identifier

- sequence of characters to identify variable, function, property, etc.
- can contain letters, digits, \_, $, can not start with digit, convention is to not use \_ or $ as they are used by external modules
- cannot be keyword, e.g. `true`, `var`, `function`, `if`, `return`, etc.
- convention is to use lower camelCase for variables, functions and object properties, and upper CamelCase for constructors and classes
- case sensitive



## Variables

- named reference of storage address in memory
- can be declared and initialised

### Declaration

- single: `keyword identifier;`
- multiple: `keyword identifier1, identifier2;`
- non-initialised variables are automatically initialised to `undefined` ❗️

### Initialisation

- single: `identifier = value;`
- multiple: `identifier1 = identifier2 = value;`

### Declaration and initialisation

- single: `keyword identifier = value;`
- multiple: `keyword identifier1, identifier2 = value;`

### Variable types

#### `var` 👎

- only function scope
- if not initialised is automatically initialised to `undefined`
- can be accessed before declaration with value `undefined`, see Hoisting
- can be redeclared in same scope

#### `let` 👍

- function and block scope
- if not initialised is automatically initialised to `undefined`
- can not be accessed until declaration, temporary death zone, see Hoisting
- can not be redeclared in same scope

#### `const` 👍

- function and block scope
- must be initialised when declared, cannot be reassigned
- beware: mutable assignments is not the same as mutable values, i.e. a const variable can't be reassigned but the value can still be mutated in case it is an object (or array, function, etc.) ❗️
- can not be redeclared in same scope

### Global Object

- predefined object in global scope
- can't access since has no own name, in sloppy mode can use `this` in global scope, in Browsers can use `window`
- all `var` and `function` declarations in global scope become properties of the global object, one of the bad quirks of JS ❗️
 
### Scope

- see Scope in Functions

### Hoisting

- see Hoisting in Functions

### Destructuring assignment

- unpack values from arrays and object properties into distinct variables (with ES6 and ES2018 respectively)
- can use in variable assignment or as function parameters in function definition, e.g. to implement named arguments
- beware: LHS is a destructuring pattern, not an array or object itself, even though it looks like it ❗️

```javascript
// basic array destructuring
const [a, b] = [1, 2, 3, 4, 5];
console.log(a); // 1
console.log(b); // 2

// using default values
const [a = 21, b = 42] = [1];
console.log(a); // 1
console.log(b); // 42

// ignoring values
const [a, , , b] = [1, 2, 3, 4];
console.log(a); // 1
console.log(b); // 4
```

- array destructuring can be used to swap variables, e.g. `[a, b] = [b, a]`
- in object destructuring variable order doesn't matter, also doesn't need to fill gaps for ignored properties like in array
- if object doesn't have own property, it's prototype chain is looked up ❗️
- default values are used only if value is `undefined`, i.e. not if `null` or any other falsy value ❗️

```javascript
// new variable names
const {a: x, b: y} = {a: 1, b: 2, c: 3};
console.log(x); // 1
console.log(y); // 2

// shorthand if variable name and property name are the same, only works for valid identifier ❗️
const {a, b} = {a: 1, b: 2, c: 3};
console.log(a); // 1
console.log(b); // 2

// default values
const {a = 21, b = 42} = {a: 1};
console.log(a); // 1
console.log(b); // 42

// new variable names and default values
const {a: x = 21, b: y = 42} = {a: 1};
console.log(x); // 1
console.log(y); // 42
```

- when object destructuring into already existing variables need to wrap whole assignment in parentheses to not be interpreted as block

```javascript
let a, b;
[a, b] = [1, 2, 3, 4, 5];
console.log(a); // 1
console.log(b); // 2

let a, b;
({a, b} = {a: 1, b: 2, c: 3});
console.log(a); // 1
console.log(b); // 2
```

- if destructures something that doesn't exist gets value `undefined`

```javascript
const [a, b, c] = [1, 2];

console.log(a); // 1
console.log(b); // 2
console.log(c); // undefined
```

- assignment returns entire value that was subject to assignment, no matter how much was destructured ❗️

```javascript
let a, b;
const x = [a, b] = [1, 2, 3];

console.log(a); // 1
console.log(b); // 2
console.log(x); // [1, 2, 3]
```

- can nest destructuring patterns, i.e. destructure nested values, e.g. from JSON data

```javascript
const o = [ { name: "Peter", age: 42 }, { name: "Laura" }, { name: "Bernd"} ];

const [
  {
    name: firstName,
    age: firstAge = 1
  },
  {
    name: secondName,
    age: secondAge = 21
  }
] = o;

console.log(firstName);  // "Peter"
console.log(firstAge);   // 42
console.log(secondName); // "Laura"
console.log(secondAge);  // 21
```

- can destructure object property multiple times, useful for objects themselves such that can have once objects and once its content

```javascript
const o = {
  a: 1,
  b: {
    c: 3
  }
}

const { a, b, b: { c } } = o;

console.log(a); // 1
console.log(b); // { c: 3 }
console.log(c); // 3
```

- beware: give default values in nested destructuring patterns incase outer pattern doesn't exists, otherwise gets `TypeError` trying to access properties of `undefined`

```javascript
const o = {};

const { a: { b = 1, c = 2 } } = o; // Uncaught TypeError: Cannot read property 'b' of undefined
const { a: { b = 1, c = 2 } = {} } = o;

console.log(b); // 1
console.log(c); // 2
```

- beware: put default values always inline instead of within another default object, because default object is applied only when whole property is missing, always leave default object empty, same applies to array ❗️

```javascript
const o = {};

const { a: { b, c } = { b: 1, c: 2 } } = o;

console.log(b); // 1
console.log(c); // 2

const o = { a: {} };

const { a: { b, c } = { b: 1, c: 2 } } = o;

console.log(b); // undefined
console.log(c); // undefined

const o = {};

const { a: { b = 1, c = 2 } = {} } = o;

console.log(b); // 1
console.log(c); // 2

const o = { a: {} };

const { a: { b = 1, c = 2 } = {} } = o;

console.log(b); // 1
console.log(c); // 2
```

- can destructure directly into another structure

```javascript
const x = [];

[x[0], x[1]] = [1, 2, 3, 4, 5];
console.log(x); // [1, 2]

const x = [];

({a: x[0], b: x[1]} = {a: 1, b: 2, c: 3, d: 4, e: 5});
console.log(x); // [1, 2]
```

```javascript
const x = {};

[x.first, x.second] = [1, 2, 3, 4, 5];
console.log(x.first); // 1
console.log(x.second); // 2

const x = {};

({a: x.first, b: x.second} = {a: 1, b: 2, c: 3, d: 4, e: 5});
console.log(x.first); // 1
console.log(x.second); // 2
```

- can use rest syntax to unpack variable number of values

```javascript
const [a, b, ...x] = [1, 2, 3, 4, 5];
console.log(a); // 1
console.log(b); // 2
console.log(x); // [3, 4, 5]

const {a, b, ...x} = {a: 1, b: 2, c: 3, d: 4, e: 5};
console.log(a); // 1
console.log(b); // 2
console.log(x); // {c: 3, d: 4, e: 5}
```

- beware when using rest syntax and unpacking into another data structure since it might look confusing ❗️

```javascript
const x = [];

[x[0], ...x[1]] = [1, 2, 3, 4, 5];
console.log(x[0]); // 1
console.log(x[1]); // [2, 3, 4, 5]

const x = [];

({a: x[0], ...x[1]} = {a: 1, b: 2, c: 3, d: 4, e: 5});
console.log(x[0]); // 1
console.log(x[1]); // {b: 2, c: 3, d: 4, e: 5}
```

```javascript
const x = {};

[x.first, ...x.second] = [1, 2, 3, 4, 5];
console.log(x.first); // 1
console.log(x.second); // [2, 3, 4, 5]

const x = {};

({a: x.first, ...x.second} = {a: 1, b: 2, c: 3, d: 4, e: 5});
console.log(x.first); // 1
console.log(x.second); // {b: 2, c: 3, d: 4, e: 5}
```



## Data types

- set of values, groups "similar" ones
- defines its possible values and operations, e.g. a boolean can only have the two values `true` and `false`
- JS is dynamically typed, i.e. no need to specify data type of variable, can assign other as well, but values still have a type

### Primitive data types

- fundamental data types
- data is immutable, can not be changed  
  beware: don't confuse data with variable, variable can be changed, but data itself like `"Hello"` can not be changed
- JS has 7 primitive data types
  - Boolean: true and false
  - Number: integer or floating point number
  - BigInt: integer with arbitrary precision
  - String: sequence of characters, text
  - Symbol: unique identifiers (with ES6)
  - `null`: empty value
  - `undefined`: missing value
- beware: `undefined` is a badly chosen name for a missing value, i.e. a variable with value `undefined` is not not defined, but instead was defined to have the value `undefined` ❗️
- beware: `undefined` is used as default missing value by language, use `null` in own code so can differentiate between own bottom value and bottom value of language ❗️

### Composite data types

- data type made up of primitive data types
- data is mutable, can be changed, e.g. item of array
- JS has 1 composite data type
  - Object: container for data
- all primitive data types except null and undefined have associated object types, e.g. `String` for strings, the `valueOf()` method of those objects returns the primitive value (see later Objects)

### Conversion

- coercion: JS automatically converts data types to expected data type whereever needed, might have unexpected outcomes

<!-- ToDo: expand -->

### Literals

- notation to represent fixed value in source code "literally", e.g. `45`, `"Hello"`, `[1, 2, 3]`
- often says data type to refer to literal of said data type, e.g. `"Hello"` is a string (literal)
- always use literals instead of constructor functions, they don't work the same, i.e. `Boolean`, `Number`, `String`, `BigInt`, `Array`, `Object`, `Function`

#### Boolean literals

`true` and `false`

#### Number literals

##### Integer literals

- decimal: `d_i \ldots d_0` with `d_i \in \{0,1,\ldots,9\}`, e.g. `117`, no leading zero
- hexadecimal: prefix `0x` or `0X`, e.g. `0x1123`
- octal: prefix `0o` or `0O`, e.g. `0o77`
- binary: prefix `0b` or `0B`, e.g. `0b11`

##### Floating-point literals

- decimal: `+`/`-d_i \ldots d_0.d_i \ldots d_0E`/`e+`/`-d_i \ldots d_0` with `d_i \in \{0,1,\ldots,9\}`, e.g. `-4.20e6`, no leading zero

#### BigInt literals

- same as integer literals, just with suffix `n`, e.g. `117n`

#### String literal

- any character(s) enclosed in quotes, single or double, but of same type, e.g. `"Hello World!"`
- template strings: interpolated literals, back-ticks instead of quotes, e.g. `` `Hello World!` ``, can run over multiple lines, can do string interpolation using `${expression}`, e.g. `` `${myStr.includes('cool') ? 'hay' : 'nay'}` `` (with ES6)
- needs to escape special characters with backslash, e.g. `\n`, `\\` or `\"`
- UTF-16 code unit `\uXXXX` for hex digits `X`  
  UTF-32 code unit `\u{X...X}` for hex digits `X`

#### Array literals

`[item1, item2, item3]`

- empty places get interpreted as undefined
- the last trailing comma is ignored if the place before was empty

#### Object literal

`{key1: value1, key2: value2}`

- key must be valid identifier or enclosed in quotes



## Conditional statements

### if...else statement

```javascript
if (condition) {
  statement1;
} else {
  statement2;
}
```

- can add `else if` statement(s) in middle, only first true condition will be executed, can also omit `else`
- any data type in Boolean context is converted to Boolean
- falsy values: values that are converted to false  
  `undefined`, `null`, `false`, `0`, `NaN`, `""`
- truthy values: values that are converted to true  
  everything else, i.e. especially `true`, `1`, `" "`, `"0"`, `[]`, `{}`, `function() {}`, _any_ object, etc.
- condition can be any expression because of truthy / falsy evaluation
- can run if statement with condition that variable is non-empty, e.g. `if (variable) {...}`

### switch statement

```javascript
switch (expression) {
  case match1:
    statement1;
  case match2:
    statement2;
  default:
    statement;
}
```

- expression gets compared strictly to cases, i.e. uses `===`
- any matching case gets executed on after the other
- stop switch with `break` keyword in each case
- if `break` is forgotten, will run all following cases until next `break` regardless if criterion was met, "falling through" ⚠️
- `default` is executed if no match is found, doesn't need to be last case, if no `break` begins to fall through into any following cases ⚠️

### try...catch statement

```javascript
try {
  statement1;
} catch (exception) {
  statement2;
}
```

- try block contains the statement to execute
- catch block contains the statement to execute as soon as try throws an exception, if no exception is skipped
- catch block is passed the exception as argument
- optional `finally` clause, runs always after `try...catch` complete, regardless of result, e.g. for clean up



## Loops

### labeled statement

```javascript
label: {statement;};
```

- label must be valid identifier
- used to reference outer loop, such that can break out / continue from within nested loop

### break statement

```javascript
break;
break label;
```

- terminates / "breaks out of" inner most enclosing statement
- using label terminates the whole labeled statement

### continue statement

```javascript
continue;
continue break;
```

- skips inner most enclosing statement to next iteration
- using label skips the whole labeled statement

### for statement

```javascript
for (initialExpression; condition; incrementExpression) {
  statement;
}
```

- condition is tested before each loop
- increment expression is executed at end of each loop

### while statement

```javascript
while (condition) {
  statement;
}
```

- like for loop, except:
- initial expression is defined outside beforehand
- increment expression is part of statement

### do...while statement

```javascript
do {
  statement;
} while (condition);
```

- like while loop, except:
- condition is checked after each loop, i.e. runs at least once

### for...in statement 👎

```javascript
for (const property in object) {
  statement;
}
```

- loops over all enumerable property _names_ of an object
- includes inherited enumerable properties ❗️
- doesn't necessarily iterate in order, implementation dependent
- use only for debugging to see all properties

### for...of statement 👍

```javascript
for (const value of object) {
  statement;
}
```

- loops over iteration sequence of an iterable object, see Iterators and Generators
- e.g. for array is simply elements in order



## Operators

- predefined functions with special names and no calling parentheses

### Assignment operators

`=`, `+=`, `-=`, `*=`, `/=`, `%=`, `**=`, (`<<=`, `>>=`, `>>>=`, `&=`, `^=`, `|=`)

- an assignment expression returns the operand, e.g. in `a = b = 5` the expression `b = 5` returns `b`

```javascript
const obj = {};

// usual
obj["world"] = 42;
obj[42] = "world";

// using return value of assignment
obj[obj["world"] = 42] = "world";
```

- exponentiation operator (with ES2016)
- can use destructuring to unpack data from objects and arrays, e.g. `let [a, b, c] = [1, "Hi", function() {...}]`, see Destructuring assignment

### Comparison operators

`==`, `!=`, `===`, `!==`, `>`, `<`, `>=`, `<=`

- all comparison operators except `===` and `!==` use automatic type conversion if operans are not of the same type
- beware: due to various historical bad parts, `==` operator is _not_ transitive ⚠️

```javascript
0 == ""           // true
0 == "0"          // true
"" == "0"         // false

false == "false"  // false
false == "0"      // true
" \n\r\s " == 0   // true
```

<!-- ToDo: expand from DJF -->

### Arithmetic operators

`+`, `-`, `*`, `/`, `%`, `**`, `--`, `+`, `-`

- only for numeric data types, i.e. Number, BigInt
- divide by zero produces `Infinity` object
- decrement operators: prefix, returns value after subtracting / adding `1`  
  increment operators: postfix, returns value before subtracting / adding `1`
- exponentiation operator (with ES2016)

### Bitwise operators

`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`

### Logical operators

`&&`, `||`, `??`, `!`

- converts values according to truthy and falsy
- short-circuit evaluation is used
- `&&` returns leftmost falsy operand, last one if both are truthy
- `||` returns leftmost truthy operand, last one if both are falsy
- `??` returns leftmost non-nullish operand, last one if both are nullish (with ES2020)
- i.e. `&&` and `||` return one of the compared values, not a boolean ❗️
- i.e. `??` behaves like `||` but only `undefined` and `null` are treated as "falsy", and not `false`, `0`, `NaN`, `""`  🎉
- use `&&` to guard value, i.e. check for null objects, e.g. `let name = obj && obj.name` is `obj.name` if `obj` exists, else `null` since it returns `obj` which is null, can replace if statement in assignment or return statement
- use `||` to default value, e.g. `let name = user || "stranger"`, can replace if statement in assignment or return statement  
  beware: also defaults if value is `false`, `0`, `NaN`, `""` since those are falsy as well ❗️
- use `??` to default value, e.g. `let name = user || "stranger"`, can replace if statement in assignment or return statement, fixes problem of `||` using default value also for `false`, `0`, `NaN`, `""`
- prepend value with `!!` as shorthand to convert to boolean
- precedence: `!` > comparison operators > `||` > `&&`
- to use `??` together with `&&` or `||` is required to use parentheses to indicate precedence ❗️

### String operators

`+`, `+=`

- any value will be converted to string first, e.g. `1 + 2 + "3"` will be `123` and not `33`

### Conditional / ternary operator

`condition ? val1 : val2`

- if condition is truthy, returns `val1`, if falsy returns `val2`
- i.e. returned value is not the condition ❗️, e.g.

```javascript
// parentheses around ternary operator just for ease of understanding, not needed
let result = (expression ? value1 : value2);        // doesn't assign expression!
let status = (age >= 18 ? "adult" : "minor");       // doesn't assign age!
() => {return (username ? "Welcome" : "No entry")}; // doesn't return username!
```

- often used to replace simple if...else statement, e.g. when checking for non-emptiness of variable
- don't use ternary operator if expression is used as one value as well, use simpler `&&` or `||` instead, e.g. when checking for null objects

```javascript
a ? a : b = a || b;
a ? b : a = a && b;
```

- can chain to create more complex conditions

```typescript
function compare(x, y): -1 | 0 | 1 {
  return x === y ? 0 : x > y ? 1 : -1;
}
```

### delete operator

`delete identifier`

- deletes own property from an object
- returns false if property is non-configurable, else true  
  beware: returns true even if property doesn't exist ❗️
- doesn't actually free any memory, this is done by garbage collection automatically
- deleted property can still "exist" if an identically named property is inherited
- doesn't correct `length` property of an array, use `Array.prototype.splice()` method instead
- doesn't work for prebuilt things, only self-defined things

### typeof operator

`typeof (expression)`

- returns type as string "object", "function", "number", "string", "boolean", "undefined", i.e. when using in condition doesn't need strict equality since only returns string ❗️
- beware: arrays and null are confusingly both "object", use `Array.isArray()` and `=== null` for null ⚠️

### void operator

`void (expression)`

- evaluates expression without returning a value, i.e. returns undefined
- can be used in links, e.g. `<a href="javascript: void(document.form.submit())">...</a>`

### in operator

`property in object`

- returns true if property is in object / index is in array

### instanceof operator

`object instanceof constructor`

- tests if object inherits from given object type, i.e. if constructor is ancestor
- checks if constructor's `prototype` property appears in the object's prototype chain, i.e. `object.[[prototype]].....[[prototype]] == constructor.prototype` for one or more `[[prototype]]`s "chained"

### ellipsis operator

`...`

#### spread syntax

- expands iterable into multiple elements, e.g. as arguments in function call or items in array literal, iterable can be array, string, etc.
- expands object's own enumerable properties in object literal (with ES2020), i.e. no methods
- i.e. no need to use `Function.prototype.apply()` anymore to pass array as arguments to function, no need to use `Array.prototype.concat()` anymore to concat arrays, etc.

```javascript
function sum(a, b, c) {
  return a + b + c;
}
const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6

const a = [3, 4];
const b = [1, 2, ...a, 5];

const a = {x: 1, y: 2};
const b = {...a, z: 3};
```

- later objects properties overwrite earlier ones ❗️

```javascript
const a = {x: 1, y: 2, z: 3};
const b = {...a, z: 4}; // {x: 1, y: 2, z: 4}
```

#### rest syntax

- condenses arbitrary number of arguments into array in parameter of function definition or destructuring assignment
- condenses arbitrary number of object own enumerable properites into object in object destructuring assignment (with ES2020)
- rest parameter must always be single last parameter

```javascript
function sum(...args) {
  return args.reduce((accumulator, currentValue) => accumulator + currentValue);
}

console.log(sum(1, 2, 3));     // 6

let [a, ...b] = [1, 2, 3];
console.log(b);               // [2, 3]

let {x, ...b} = {x: 1, y: 2, z: 3};
console.log(x);               // 1
console.log(b);               // {y: 2, z: 3}
```

- spread syntax is opposite of rest syntax, former destructures arrays and objects into elements and properties, latter condenses elements and properties into arrays and objects, former is in argument of function call, latter in argument of function definition
- use rest and spread syntax to hand through arguments to a function

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;  
  }
}

function newPerson(...args) {
  return new Person(...args);
}

const p = newPerson("Peter", 42);
```

### Other operators

- `()`: grouping operator, controls precedence of expression evaluation
- `.`, `[]`, `?.`: property accessors, access properties of object, see Objects
- `new` operator, see Objects
- etc.



## Strict Mode

- directive that changes behavior of JS
- attempt to correct bad design decisions and syntax
- throws more errors instead of silently ignoring mistakes
- allows for more optimization and security
- all new projects should use strict mode ❗️

### Enabling

- can either apply to global scope or function scope, not to block scope
- place `"use script";` at top of script / function
- only comments are allowed to come before
- since it's a string, it's ignored by incompatible browsers  
  beware: if code relies on different behavior in strict mode, might break on incompatible browsers ⚠️
- contents of modules and classes use strict mode by default
- beware: combining multiple scripts where not all use strict mode into a single file could break things ⚠️

### Changes

#### New errors

- assignment to non-writable global variable, e.g. `NaN`, `undefined`, `Infinity`, etc.
- assignment to non-writable object property
- assignment to getter property of object
- creating new property on non-extensible object
- deleting non-configurable object property
- deleting variable or function
- setting an undeclared variable, (before a global variable was declared implicitly as if had used `var` in global scope)
- duplicate function parameters or object literal property names
- using the `callee` or `caller` property of the `arguments` object in a function
- using the `caller` or `arguments` property of a function object
- a number with zero as leading digit, to deprecate old octal syntax
- setting properties on primitive values
- using `with` keyword
- using more reserved keywords, e.g. `eval`, `arguments`, `private`, `static`, etc.

#### New behavior

- function declarations are scoped to block instead of containing function, e.g. conditional function declarations are not implementation dependend anymore 🎉 (initially in ES5 strict mode were only allowed at top-level of the scope, i.e. forbidden in if statement, for loop, etc., ES6 allowed again by introducing the scope limitation)

```javascript
"use strict";
if (true) {
  function x() {return 42;}
}
else {
  function x(){ return 21;}
}
x(); // ReferenceError: x is not defined
```

- `arguments` object doesn't alias function parameters anymore, i.e. changing one doesn't affect the other
- `eval` doesn't add variables to surrounding scope anymore, only accessible for code being evaluated
- `eval` evaluates code in strict mode as well
- `this` in a function is `undefined` by default instead of the global object, i.e. forgetting `new` keyword before constructor will not silently modify global object anymore 🎉
- `this` in a function called by `Function.prototype.apply`/`call()` is the actual value passed instead of it being wrapped as object, (before `null` and `undefined` were even wrapped as global object)
- the default value for `this` in `Function.prototype.apply`/`call()` is `undefined` instead of the global object



## Misc

### Polyfills

- piece of code to provide spec compliant functionality that is not yet supported on the runtime, e.g. classes on runtimes that don't yet support ES6, etc.
- if uses transpiler is added automatically based on desired target environment, e.g. Babel
- JavaScript polyfill conditional check

```javascript
if(X.prototype.hasOwnProperty('Y') {
  // definition of Y
}
```



## Resources

### General

- MDN - [JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- Wikipedia for terminology
- Douglas Crockford - Javascript: The Good Parts
- Kyle Simpson - [Deep JavaScript Foundations, v3](https://frontendmasters.com/courses/deep-javascript-v3/)

### Specific

- Ben Cherry - [JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
- Fabrício S. Matté - [Temporal Dead Zone (TDZ) demystified](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified)
- [V8 - Nullish coalescing](https://v8.dev/features/nullish-coalescing)