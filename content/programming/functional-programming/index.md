---
title: Functional Programming
author: vwkd
index: 3.1
tags:
  - programming
---

<!-- ToDo: FINISH -->

- a declarative programming paradigm
- computation as the evaluation of mathematical functions
- use pure functions as much as possible, minimise and contain impurities, e.g. extract into other function or wrap in function
- gives predictability, since outcome of pure functions is mathematically provable
- engine can optimise pure functions since don't have side effects, e.g. run in parallel, "memoize" return values for further invocations, etc.



## Concepts

- function (in FP): relation between input and output in mathematical sense, i.e. when can write down mathematically like `f(a, b) = a * b`  
  every other function is called "procedure", a function calling a procedure becomes itself a procedure

```javascript
// function
function square(x) {
  return x**2;
}

// procedure
function print(x) {
  console.log(x);
}

// procedure, since calls procedure
function squareLog(x) {
  print(x);
  return x**2;
}

// procedure, since object can mutate
function getId(object) {
  return object.id;
}
```

- pure function: function that has no side effects when evaluated, i.e. given same input always produces same output, computational analogue of a mathematical function, provable

```javascript
function multiply(a, b) {
  return a * b;
}
```

- impure function: function that is not pure, a function calling an impure function becomes itself impure, e.g. modifying object, its own arguments, etc.
  beware: a dependency on a non-local variable doesn't necessarily make the function impure, it could just be a constant placeholder variable, only if its value is uncertain

```javascript
const b = Math.random();

function multiply(a) {
  return a * b;
}
```

- multidimensional functions can be implemented using arrays

```javascript
function move(x, y, z) {
  return [x+1, y+1, z+1];
}

const [newX, newY, newZ] = move(3, 3, 3);
```

- higher-order functions: functions that take other functions as arguments or returns a function as result, e.g. `Array.prototype.forEach`/`.map`/`filter`/`reduce` etc.  
  requires that functions are first-class, is given in JavaScript


## TODO TODO TODO




- can return nested functions, still pure functions, just two

```javascript
// both are pure function calls

function addMaker(x) {
  return function(y) {
    return y + x;
  };
}

const add5 = addMaker(5);
const total = add5(15);
```

- shape of function: signature, `unary`, `binary`, `n-ary` about how many inputs it takes
  most programmers prefer `unary` functions, then `binary` ???


- point-free: define function without needing to define it's points (i.e. inputs), i.e. making functions from other functions, pass function as argument without specifying its arguments because the function expects a function like that  
  is declarative instead of imperative  
  like math function compositions ??? `f = g + h`

```javascript
// pure function
function sum(x, y) {
  return x + y;
}

// point-free function ???
function mysum(fn) {
  fn(42, 21);
}

// not point-free
mysum((x, y) => sum(x, y));
// point-free
mysum(sum);
```

- define functions in relationship between two function to make relationship obvious, e.g. define `isEven` as negation of `isOdd` instead of on its own

```javascript
function isOdd(x) {
  return x % 2 == 1;
}

// isEven is not point-free because it takes input x 👎
function isEven(x) {
  return !isOdd(x);
}
```

```javascript
function not(fn) {
  return function(...args) {
    return !fn(...args);
  }
}

function isOdd(x) {
  return x % 2 == 1;
}

// isEven is point-free because it hides input x 👍
const isEven = not(isOdd);
```

## Closure

- can use closure in pure function, as long as doesn't change the closed over variable ❗️

```javascript
function counter() {
  let i = 0;

  return function () {
    index += 1;
    return index;
  }
}

const generator = counter();

generator(); // 1
generator(); // 2
generator(); // 3
```

## Composition

## Immutability

## Recursion

## Lists / Data Structures

## Async

## FP Libraries

- contain functions that change the shape of functions, e.g. flip first two parameters, make unary or binary, reverse parameters, etc.




# TODO

curry composition


Higher-order functions enable partial application or currying, a technique that applies a function to its arguments one at a time, with each application returning a new function that accepts the next argument. This lets a programmer succinctly express, for example, the successor function as the addition operator partially applied to the natural number one.