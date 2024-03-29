---
title: Functions
author: vwkd
index: 4
tags:
  - languages
  - javascript
---

- special objects: callable, additional `prototype` property, constructor of a function is `Function`, inherits some properties
- can have properties and methods like any other object
- used to bundle code, hide information 
- are first-class in JS, can treat them like any other value since they are just objects, see later with Function expression, i.e. pass as argument, return, assigning to variable, storing in object, etc.
- returns `undefined` by default, can change with `return` keyword



## Function definition

- comprised of name as valid identifier, parameters in parentheses comma-separated, and body in curly brackets containing the statements
- function parameters are in definition, function arguments are in call, arguments technically get assigned to parameters
- can give parameters a default value using `param1 = val1`
- parameters are not mandatory, if not provided are `undefined`, if more provided are ignored
- can access all parameters through array-like `arguments` object  
  beware: in non-strict mode `arguments` object's elements are synchronized with parameters, changing either one changes the other, use strict mode to not synchronise ❗️
- can use rest syntax to capture multiple arguments in an array, see [Ellipsis operator](#)
- can use destructuring of arguments, see [Destructuring assignment](#)
- beware: don't use `Function` constructor to create a function, creates in global scope only, i.e. don't have closures, is parsed only on creation instead of in advance, doesn't allow for optimisation because function body is string, slow ❗️

### Function declaration / statement

- function as a statement, in main code flow

```javascript
function identifier(param1, param2) {
  statements;
}

identifier(arg1, arg2);
```

- name is mandatory
- does not end with a semicolon
- in non-strict mode is scoped to containing function, in strict mode to containing block

```javascript
// containing scope is function a
function a() {
  function b() {
    console.log("Hello World!");
  }
}
b(); // ReferenceError: b is not defined

// containing scope is main code, since non-strict mode
{
  function b() {
    console.log("Hello World!");
  }
}
b(); // "Hello World!"

// containing scope is block, since strict mode
"use strict";
{
  function b() {
    console.log("Hello World!");
  }
}
b(); // ReferenceError: b is not defined
```

- hoisted to top of containing scope, i.e. can use function before declared it, see hoisting
- behaves as if was defined using `var` function expression except that body is also hoisted, i.e. if defined in global scope becomes property of global object, one of the bad qirks of JS ❗️
- beware: don't declare functions in conditional block in non-strict mode, behavior differs across implementations, instead use function expressions and/or strict mode ❗️

```javascript
if (true) {
  function x() {return 42;}
}
else {
  function x(){ return 21;}
}
x(); // 42 in FF 72, Chrome 79
      // 21 in Safari 13
```

### Function expression

- function as an expression, not in the main code flow, e.g. in variable assignment, function argument, return statement, object literal, etc.
- not hoisted ❗️(also in variable assignment only variable itself is hoisted)
- if used as an assignmend, is scoped to containing function or block depending on `var`, `let`, or `const`

#### Named

- name is only accessible within the function expression, e.g. for recursion, also helpful because it shows up in stack traces

```javascript
// named function expression in assignment
const a = function identifier(param1, param2) {
  statements;
};
```

#### Anonymous

- name is optional, function expression can be "anonymous"

```javascript
// anonymous function expression in assignment
const a = function (param1, param2) {
  statements;
};
  ```

- always give name, more self-documenting, also shows up in stack trace, name is inferred in assignment but nowhere else, e.g. in IIFE ❗️

#### Arrow function

- shorthand for anonymous function expression (since ES6)
- use mindfully since are anonymous, stack trace is anonymous ❗️

```javascript
// arrow function in assignment
const identifier = (param1, param2) => {statements;};

// shorthand for direct return statement
                            /*...*/ => expression; // equivalent to {return expression;}

// shorthand for single parameter
                             param1 => /*...*/     // equivalent to (param1)

const multi = (a, b) => a*b;    // shorthand for (a, b) => {return a*b;}
const double = x => x*2;        // shorthand for (x) => {return x*2;}
const sayHello = () => {console.log("Hello World!")}
```

- direct return shorthand interferes with block body syntax when returning object literals, wrap in parentheses instead, e.g. `() => ({x: 1})`
- does not have a `prototype` property, i.e. can't be used as constructor with `new`
- does not have a `this`, `arguments`, or `super` object
- takes `this` from enclosing scope _at site of definition_, like any other non-local variable, not bound to parents `this` but whatever enclosing scope it is defined in ❗️
- can use arrow function as function inside a method without needing to bind `this`, but not as method itself because `this` would be global object ❗️ 

```javascript
// inside constructor 'this' context is newly created object
function Person(name) {
  this.name = name;
  this.sayHi = function() {
    console.log("Hi, my name is " + this.name);};
  this.sayHi2 = () => {
    console.log("Hi, my name is " + this.name);};
}

const p = new Person("Peter");
p.sayHi();  // Hi, my name is Peter
p.sayHi2(); // Hi, my name is Peter

const q = {
  name: "Sarah",
  sayHi: p.sayHi,
  sayHi2: p.sayHi2
};

q.sayHi(); // Hi, my name is Sarah
q.sayHi2(); // Hi, my name is Peter

// Note: In practice don't put this-aware methods on instance, doesn't make sense because can just access the variables directly without going through dynamic this, use the prototype chain instead
```

```javascript
// outside constructor 'this' context is global object / undefined
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function() {
  console.log("Hi, my name is " + this.name);};
Person.prototype.sayHi2 = () => {
  console.log("Hi, my name is " + this.name);};

const p = new Person("Peter");
p.sayHi(); // Hi, my name is Peter
p.sayHi2(); // Hi, my name is

const q = {
  name: "Sarah",
  sayHi: p.sayHi,
  sayHi2: p.sayHi2
};

q.sayHi(); // Hi, my name is Sarah
q.sayHi2(); // Hi, my name is
```



## Function call

`identifier(param1, param2)`

- can use spread syntax to call with multiple values from iterable, see ellipsis operator
- arguments are passed by value, for objects the object reference is passed as value, i.e. outside objects can be modified from within the function ❗️
- `this` is determined by how the function is called, see `this` keyword, call with `Function.prototype.call()` to specify `this`, also `Function.prototype.apply()` takes array of arguments
- callback: function that is passed as argument into another function, is "called back" later in this other function
- to pass function itself, no parentheses, using parentheses calls it and returns only value, e.g. when creating a method from existing function, attaching event handler, etc.
- use single object as function parameter that contains all parameters as properties, allows to later add / remove parameters without breaking old function calls, no need to order

#### IIFE (Immediately Invoked Function Expression)

- immediately invoked function expression
- utilise the powers of functions without reusing it, single-use function
- can be used to create scope, like a block but works also for `var`, e.g. to hide variables from outer scope, to correct reference, see later Closure, function declaration would still "pollute" the enclosing scope and could be called again
- parentheses needed to distinguish it from invalid function declaration
- always give a name so it is easier to debug in stack trace ❗️

```javascript
(function myIFEE(param1, param2) {
  statements;
}(param1, param2))
```

- can be used to create an expression from anything, e.g. variable assignment

```javascript
const = (function tryAssign() {
  try {
    return 42;
  } catch(e) {
    return 21;
  }
}());
```



## Scope

### Introduction

- name-binding: the association of an identifier to an entity in memory, e.g. name of variable, name of function
- scope: region of the source code where a name-binding is valid, i.e. region where a variable can be referenced using a given identifier
- in different scopes the same identifier can refer to different entities, i.e. can have different name-bindings for same identifier ❗️
- can think of scopes as buckets that store name-bindings, dictionaries where can look up name-bindings
- (we refer here only to lexical / static scope where scope is determined by _declaration_ of name-binding in source code, by contrast dynamic scope is the region in the source code where a name-binding is in memory during execution, few language like bash use it)

```javascript
let x = 42;

function b() {
  console.log(x);
}

function a() {
  let x = 21;
  b();
}

a(); // 42 with lexical scope, 21 with dynamic scope
```

- global scope is outer most scope, i.e. top-level outside any blocks and variables
- undeclared name-binding: not declared in any scope to which has access to  
  undefined name-binding: declared in a scope to which has access to, but without a value, or rather value `undefined`
- local variable: variable that is defined in the current scope  
  non-local variable: variable that is not defined in the current scope, most time in outer scope

### Using scope

- functions create scope

```javascript
function a() {
  const x = 42;
  console.log(x);
}

a();            // 42
console.log(x); // Uncaught ReferenceError: x is not defined
```

- since ES6 also blocks create scope for `const` and `let` variables

```javascript
if (true) {
  const x = 42;
  console.log(x); // 42
}

console.log(x);   // Uncaught ReferenceError: x is not defined
```

- beware: JS has block scope-like syntax using curly braces, but only `let` and `const` are block scoped, not `var` or functions, e.g. `i` in `for (var i = 0; ...) {...}` is a global variable, keeps existing in outside scope, prefer `let` and `const` at all times ⚠️

```javascript
for (var i = 0; i < 42; i++) {
    function a() {
      console.log("Hello World!");
    }
}

a(); // Hello World!
console.log(i); // 42
```

- in nested scopes an inner scope has access to the outer scope, but outer scope doesn't have access to inner, i.e. inner scope has access to all outer scopes until global scope because of recursive argument

```javascript
var x = 42;

function a() {
  var y = 21;
  
  function b() {
    var z = 1;
    console.log(x, y, z); // 42, 21, 1
  }
  b();
  
  console.log(x, y); // 42, 21, z not defined
}
a();

console.log(x); // 42, y, z not defined
```

- shadowing: if uses the same identifier in nested scopes, can't reference outer name-binding from inner scope since inner name-binding "shadows" outer, avoid duplicate names to minimise confusion ❗️

```javascript
let x = 42;

function a() {
    let x = 21;
    // can't access the outer 'x' here since shadowed by inner 'x'
    console.log(x); // 21
}

console.log(x); // 42
```

- use IIFE to create scope that doesn't pollute outer scope with a name-binding, or block for `const` and `let`

```javascript
(function() {
  var x = 42;
  console.log(x); // 42
}());

console.log(x); // Uncaught ReferenceError: x is not defined
```

- different entities with the same identifier in different scopes are different

```javascript
// global scope

function a() {
  // scope of a
  var x = 42;
  console.log(x); // 42
}
a();

console.log(x); // ReferenceError: x is not defined

function b() {
  // scope of b
  var x = 21;
  console.log(x); // 21
}
b();
```

- a scope does not exists only after the declaration of the name-binding, i.e. variables and functions are "hoisted" to top of containing scope, see [Hoisting](#)

### Understanding scope

#### Compilation

- scopes are created during compilation / parsing, not during execution
- for each _declaration_ of a name-binding in a scope an entry is saved in that scope
- duplicate declarations are ignored

```javascript
// 'x' is added to current scope
let x = 21;
// 'a' is added to current scope
function a() {}
// nothing is added to scope since already exists
let x = 42;
function a() {}
// nothing is added to scope since no declaration
x = 99;
console.log(x);
a();
z = function() {};
```

- scopes can be nested, declared name-bindings are added only to current scope ❗️

```javascript
// 'x' is added to scope of global object
let x = 21;

// 'a' is added to scope of global object
function a() {
    // 'x' is added to scope of 'a'
    let x = 42;
    console.log(x);
}

a(); // 42
console.log(x); // 21
```

- named function expressions add their name to their own scope, while function declarations add it to the enclosing scope ❗️

```javascript
// 'a' is added to scope of global object
function a() {
}

// 'b' is added to scope of global object
const b = function z() {
    // 'z' is added to scope of 'a'
}

a(); // ✅
b(); // ✅
z(); // 🚫 Uncaught ReferenceError: z is not defined
```

- a function parameter is a formal declaration of a variable in that function scope ❗️

```javascript
// 'x' is added to scope of global object
let x = 21;

// 'a' is added to scope of global object
function a(x) {
    // 'x' (parameter) is added to scope of 'a'
    x = 42;
    console.log(x);
}

a(99); // 42
console.log(x); // 21
```

#### Execution

- during execution the current scope tries to resolve each name-binding of declaration reference or retrieval reference
- if the current scope knows the name-binding, it returns the entity, can then read value from it or write value to it

```javascript
// 'x' is returned by scope of global object
let x = 21;

// skipped during execution since pure declaration, comes back only when executes a()
function a() {
    // 'x' is returned by scope of 'a'
    let x = 42;
    // 'x' is returned by scope of 'a'
    console.log(x);
}

// 'a' is returned by scope of global object
a(); // 42
// 'x' is returned by scope of global object
console.log(x); // 21
```

- if current scope doesn't know name-binding, goes to next outer scope, tries to resolve it there

```javascript
// 'x' is returned by scope of global object
let x = 21;

// skipped during execution since pure declaration, comes back only when executes a()
function a() {
    // 'x' is unknown to scope of 'a', but scope of global object returns it
    x = 42;
    // 'x' is unknown to scope of 'a', but scope of global object returns it
    console.log(x);
}

// 'a' is returned by scope of global object
a(); // 42
// 'x' is returned by scope of global object
console.log(x); // 42
```

- if name-binding is undeclared, automatically creates one in global scope ⚠️ use strict mode to instead give proper `ReferenceError`

```javascript
// skipped during execution since pure declaration, comes back only when executes a()
function a() {
    // 'x' is unknown to scope of 'a', but scope of global object returns it
    x = 42;
    // 'x' is unknown to scope of 'a', but scope of global object returns it
    console.log(x);
}

// 'a' is returned by scope of global object
a(); // 42
// 'x' is returned by scope of global object
console.log(x); // 42
```

- a function argument in a function call is an assignment to the parameter variable, even though there is no assignment operator

```javascript
// 'x' is returned by scope of global object
let x = 21;

// skipped during execution since pure declaration, comes back only when executes a()
function a(x) {
    // 'x' (parameter) is returned by scope of 'a'
    x = 42;
    // 'x' (parameter) is returned by scope of 'a'
    console.log(x);
}

// 'a' is returned by scope of global object
a(99); // 42
// 'x' is returned by scope of global object
console.log(x); // 21
```



## Hoisting

- laymans term to describe the effect of scope on name-bindings declared not on top of scope
- since scope is created at compile-time, during execution name-bindings already exist at top of scope, no matter where in the scope they are actually declared, e.g. further down
- looks like declarations of name-bindings are moved to top of the scope, "hoisted"
- but "hoisting" doesn't exist in specs of JS
- beware: only because a name-bindings exist in whole scope, doesn't mean the entity has a value in the whole scope, i.e. can not access value before declaration ❗️
- hoisting of function declarations useful for code readability, e.g. leave all helper functions at bottom to see relevant code directly at top

### Using hoisting

- declaration is hoisted, but not initialisation ❗️
- declaration is hoisted even if it would never be executed, e.g. because it's inside a non-true if statement, etc. ❗️
- var is hoisted and initialised with `undefined`, as if it was declared with `var identifier` on top of containing scope

```javascript
// normal declaration, automatic initialisation of undefined
var x;
console.log(x); // undefined

// as if "var x;" was hoisted to top, has value undefined
x = 3;
console.log(x); // 3
var x;

// as if "var x;" was hoisted to top, has value undefined
console.log(x); // undefined
var x;

// as if "var x;" was hoisted to top, has value undefined
console.log(x); // undefined
var x = 3;

// confirmation that var variables are really hoisted
// if x wouldn't be hoisted it would log "42", but it "knows" of later declaration, because it is hoisted to top of function scope
var x = 42;
(function() {
  console.log(x); // undefined
  var x = 21;
}());
```

- function declarations are hoisted in entirety with body, as as if it was declared on top of containing scope
- function declarations rule over variable declarations, but not variable initialisations
- for multiple function declarations with the same name, the latter overwrites the former, no matter if it will be executed or not, e.g. after return statement, etc.

```javascript
// as if "function y() {...};" was hoisted to top
y(); // "Hello World!"
function y() {console.log("Hello World!");};

// as if "var y;" was hoisted to top, has value undefined
y(); // TypeError: y is not a function
var y = function() {console.log("Hello World!");};

// function declaration rules over variable declaration but not variable initialisation
function y() {}
var y;
alert(typeof y); // "function"
var y = 42;
alert(typeof y); // "number"

// as if "function y() {...};" was hoisted to top
y(); // "Hello World!"
var y = 42;
function y() {console.log("Hello World!");};

// as if "function y() {...};" was hoisted to top of containing function, later definition overrode earlier, doesn't matter if code never reaches it
function x() {
  function y() {
      return 42;
  }
  return y();
  function y() {
      return 21;
  }
}
console.log(x()); // 21
```

- let, const and classes are also hoisted to top of containing scope, but not initialised, remain without a value until the declaration (instead of `undefined`), "Temporal Death Zone", i.e. can't be read or assigned before declaration ❗️

```javascript
// normal declaration, automatic initialisation of undefined
let x;
console.log(x); // undefined

// x exists but has no value, can't read or assign
x = 3; // ReferenceError: Cannot access 'x' before initialization
console.log(x);
let x;

// x exists but has no value, can't read or assign
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x;

// x exists but has no value, can't read or assign
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 3;

// confirmation that let variables are really hoisted
// if x wouldn't be hoisted it would log "42", but it "knows" of later declaration, because it is hoisted to top of function scope
let x = 42;
(function() {
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 21;
}());
```

- some challenges

```javascript
var x = 42;
(function() {
  if (!x) {
    var x = 21;
  }
  console.log(x); // 21
}())
// because x is hoisted to top of function with value undefined, the if statement executes

var x = 42;
(function() {
  x = 10;
  return;
  function x() {}
}())
console.log(x); // 42
// because x is hoisted as function declaration to top of function, it creates a separate local x that is assigned 10, so the outer x is not changed

if (!window.x) {
  var x = 42;
}
console.log(a); // undefined
// as if "var x;" was hoisted to top, has value undefined, because window.x does exists, the if statement is not run
```



## Closure

- problem: implementing nested scopes with first-class functions, because with first-class functions the inner scope can live "longer" than the outer scope, e.g. if an outer function returned an inner function, or if inner function is saved and called later, may depend on non-local name-bindings e.g. as event handler, as argument to async function like `setTimeout()`, etc.
- solution: maintain record of name-bindings from outer scopes, i.e. non-local name-bindings a function may depend on
- environment: record of name-bindings from outer scopes
- closure: record of a function together with its environment, created every time a function is created
- often refers to the environment and also the function itself as "closure"
- allows function to access variables from outer scopes even when its executed in a different scope
- an entity is only garbage collected if there are no more references to it from existing closures
- (we refer here only to lexical closure as with lexical scope, not dynamic closure as with dynamic scope)
- accessing such a non-local name-binding using closure is called "closing over that name-binding"

```javascript
// returned function depends on non-local variable x
function a() {
  const x = 42;
  return function() {console.log(x);};
}

const b = a(); // "b closes over x"
b(); // 42
```

- the name-bindings in the environment give the actual values, closure is not just a snapshot of the scope at time of creation, doesn't capture a value, instead preserves live access to variable, can not "close over" a value but only a variable ❗️

```javascript
function wrap() {
  let x = 42;
  function a() {
    return function() {console.log(x);};
  }
  x = 21;
  return a;
}

const b = wrap()();
b(); // 21
```

- using closure and IIFE can create private entities, i.e. to hide from global namespace / not clutter it

```javascript
const o = (function() {
  // state variables
  return {
    // object that depends on state variables
  };
}());

const f = (function () {
  // state variables
  return function(...) {
    // function that depends on state variables
  };
}())
```

```javascript
const b = (function() {
  const x = 42;
  return function() {console.log(x);};
}());
b(); // 42

const nameContainer = (function() {
  const names = ["Tick", "Trick", "Track"];
  return function(n) {
    return names[n];
  }
}());

function createAdder(x) {
  return function(y) {
    return x + y;
  };
}
let add5to = createAdder(5); // closure of x = 5
console.log(add5to(3)); // 8
```

- using closure can create generators, i.e. return different values each time they're called

```javascript
function factory(...) {
  // generator's state variables
  return function() {
    // update state variables and return new value
  }
}
const generator = factory(...);
```

```javascript
// without IIFE
function counter() {
  let i = 0;

  return function () {
    index += 1;
    return index;
  }
}
const generator = counter();
console.log(generator()); // 1
console.log(generator()); // 2
console.log(generator()); // 3

// with IIFE
const idGenerator = (function() {
  let id = 0;

  return function() {
      id += 1;
      return id
    }
}());

console.log(idGen()); // 1
console.log(idGen()); // 2
console.log(idGen()); // 3
```

```javascript
function element(arr) {
  let i = 0;

  return function() {
    if (i < arr.length) {
      const item = arr[i];
      i += 1;
      return item;
    }
  }
}
const generator = element(["Hello", "World", 420]);
console.log(generator()); // "Hello"
console.log(generator()); // "World"
console.log(generator()); // 420
console.log(generator()); // undefined
```

- using closure can create cascades, e.g. like jQuery, Gulp pipes, etc.

```javascript
// functional style
function foo() {
  let counter = 0;

  const bar = {
    inc() {
      counter += 1;
      return bar;
    },

    print() {
      console.log(counter);
      return bar;
    }
  };

  return bar;
}

foo()
  .inc()
  .inc()
  .print() // 2
  .inc()
  .print(); // 3
```

```javascript
// class style
class bar {
  #counter = 0;

  inc() {
    this.#counter += 1;
    return this;
  }

  print() {
    console.log(this.#counter);
    return this;
  }
}

new bar()
  .inc()
  .inc()
  .print() // 2
  .inc()
  .print(); // 3
```

### Common error

- although each function has its individual closure, its environment must not be distinct, can lead to errors when environment is mistakenly expected to be distinct
- commonly functions created in a loop depending on non-block-scoped variables, e.g. when attaching event handlers, scheduling `setTimeout` calls, etc.
- one more reason to always use block-scoped variables, or create new function scope by wrapping in IIFE

#### Example problem

- use loop to put `100` functions into an array which log `i` when called later
- problem: `i` is always `100`
- functions could have been passed to other entities that use them later, e.g. `setTimeout`, event handler, etc.

```javascript
var arr = [];
for (var i = 0; i < 100; i++) {
    arr[i] = function() {console.log(i);}; // function depending on i
}
arr.forEach(item => {item();}); // i is always 100
```

#### explanation

- because `i` is `var` and has function scope it is declared in the outer scope instead of the block scope of the for loop
- on each loop `i` is silently redeclared without an error
- because of closure each function has access to the non-local variables, which is here the single `i` variable, in other words the function's environments are all identical
- by the time the functions read the value of `i` the loop has already run and `i` has the last value `100`
- `arr[i]` works correctly because the value of `i` is evaluated at the time of assignment instead of only after the loop has finished

#### solution

- solution 1: use `let` to make `i` block-scoped
- now the function's environments all contain different `i`'s of the respective iteration

```javascript
var arr = [];
for (let i = 0; i < 100; i++) {
    arr[i] = function() {console.log(i);};
}
arr.forEach(item => {item();});
```
  
- solution 2: use IIFE to create nested function scope with local `i` (renamed to `index` for clarification)
- now the value of `i` is determined at time of the IIFE execution, similarly how `arr[i]` works

```javascript
var arr = [];
for (var i = 0; i < 100; i++) {
    arr[i] = (function(index) {
      return function() {console.log(index);}
    }(i));
}
arr.forEach(item => {item();});
```

- solution 3 (only works with arrays): use `Array.prototype.forEach()` instead of for loop, `i` is scoped to iteration like in for loop with `let`

```javascript
var arr = Array.from(Array(100));
arr.forEach((item, i) => {arr[i] = function() {console.log(i)};});

arr.forEach(item => {item();});
```



## Built-in functions

### Global functions

- `eval()`: runs JS code from string, slow and insecure, never use ⚠️
- `isNaN()`: checks if value is `NaN`, uses coercion for non-number data types, many false positives, use newer `Number.isNaN()` instead ⚠️
- `isFinite()`: false if value is `Infinity`, `NaN` or `undefined`

### `Function` object type

- `Function.prototype.call()`: calls function with specified object as `this`, arguments as list
- `Function.prototype.apply()`: calls function with specified object as `this`, arguments as array



## Resources

- MDN - as usual