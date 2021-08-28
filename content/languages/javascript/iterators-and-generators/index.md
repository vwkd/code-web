---
title: Iterators and Generators
author: vwkd
index: 9
tags:
  - languages
  - javascript
---

## Iterator

- object that produces a sequence of values
- sequence can be any finite or infinite, unlike array which is only finite
- consumer can pull out values from producer one at a time, like a source, one-way communication
- consumer is in control, producer "waits" until consumer pulls next value
- can consume only once since uses same iterator

### Consumer

- `.next()`: returns sequence elements as value-done object
    - `value`: current value of the sequence, can be omitted when `done` is `true` or be the final return value
    - `done`: `false` if there is another value in the sequence, else `true`
- `.return()` (optional): closes iterator, returns last sequence element as value-done object (`done` is `true`), e.g. can use to clean up
- `.throw()` (optional): closes iterator if unhandled, throws error if unhandled
- continuing to iterate a closed iterator gives `{ value: undefined, done: true }` value-done object

### Producer

- can create using using IIFE and closure to have internal state variables

```javascript
// infinite sequence
const counter = (function() {
    let id = 0;

    return {
        next() {
            id += 1;
            return { value: id, done: false };
        }
    };
})();

console.log(counter.next()); // { value: 1, done: false }
console.log(counter.next()); // { value: 2, done: false }
// ...
```

```javascript
// finite sequence
const it = (function() {
    let num = 0;

    return {
        next() {
            if (num <= 10) {
                return { value: num++, done: false };
            } else {
                return { value: undefined, done: true };
            }
        }
    };
})();

console.log(it.next()); // { value: 0, done: false }
// ...
console.log(it.next()); // { value: 10, done: true }
console.log(it.next()); // { value: undefined, done: true }
// ...
```

```javascript
// range [start, end)
function rangeMaker(start = 0, end = Infinity, step = 1) {
    let nextIndex = start;
    let iterationCount = 0;

    return {
        next: function() {
            let result;
            if (nextIndex < end) {
                result = { value: nextIndex, done: false };
                nextIndex += step;
                iterationCount++;
            }
            return result || { done: true };
        }
    };
}

const range = rangeMaker(0, 10);

console.log(range.next()); // { value: 0, done: false }
console.log(range.next()); // { value: 1, done: false }
// ...
console.log(range.next()); // { value: 9, done: false }
console.log(range.next()); // { done: true }
// ...
```

```javascript
// infinite sequence
const fibonacci = (function() {
    let n1 = 0;
    let n2 = 1;

    return {
        next() {
            const current = n1;
            n1 = n2;
            n2 = current + n2;

            return { value: current, done: false };
        }
    };
})();

console.log(fibonacci.next()); // { value: 0, done: false }
console.log(fibonacci.next()); // { value: 1, done: false }
// ...
```



## Iterable

- object with method under unique computed `Symbol.iterator` key that returns iterator
- used to define iteration behavior, e.g. in for...of loop, spread syntax, destructuring assignment, `yield*`, etc.
- many built-in APIs expect iterables, e.g. `Promise.all()`, `Promise.race()`, `Array.from()`, `new Map()`, `new Set()` and their "weak" counterparts, etc.
- iterator is closed automatically if terminates early anywhere, calls its `.return()` method, e.g. `break`, `return`, `throw` (uncaught) in `for...of` loop, etc.
- beware: could make iterator iterable with `Symbol.iterator` method returning itself, but would mutate itself instead of returning a copy so can only iterate over it once ❗️
- beware: values for which `done` is `true` don't show up in built-in iterations, e.g. `.return()`, etc. ❗️
- beware: to access the iterable object using `this` from within the `.next()` method on the iterator, must use an arrow function or fix `this` outside in the `Symbol.iterator` method, e.g. using `that = this` ❗️

```javascript
const p = {
    names: ["Peter", "John", "Lisa"],
    [Symbol.iterator]() {
        let i = 0;
        let length = this.names.length;

        return {
            next: () => {
                if (i < length) {
                    return { value: this.names[i++], done: false };
                } else {
                    return { done: true };
                }
            }
        };
    }
};

for (const val of p) {
    console.log(val); // Peter John Lisa
}
```

- many built-in iterables, have `Symbol.iterator` property on their prototype chain, e.g. `String`, `Array`, `Map` and `Set`, but not `Object` ❗️

```javascript
const arr = [1, 2, 3, 4, 5];

for (const val of arr) {
    console.log(val); // 1 2 3 4 5
}
```

- can overwrite iteration behavior of built-in iterables, put `Symbol.iterator` property on instance

```javascript
const arr = [1, 2, 3, 4, 5];

arr[Symbol.iterator] = function() {
    let i = 0;
    const maxIndex = this.length - 1;

    return {
        next: () => {
            if (i <= maxIndex) {
                return { value: this[maxIndex - i++], done: false };
            } else {
                return { done: true };
            }
        }
    };
};

for (const val of arr) {
    console.log(val); // 5 4 3 2 1
}
```



## Generator function

- "function" that creates iterable iterator
- nicer syntax than normal function returning an iterable iterator
- consumer can push in / pull out values to / from producer one at a time, like a sink / source, two-way communication
- consumer is in control, producer "waits" until consumer pushes / pulls next value
- can consume as often as wants since call creates new iterator
- beware: as of 2021 can't yet use for arrow function, see [Proposal](https://github.com/tc39/proposal-generator-arrow-functions) ❗️
- can use for method in object literal or class
- can't use for class constructor or getter / setter
- beware: don't pass as callback to built-in methods since won't iterate it, e.g. to `.forEach()` of array ❗️

### Producer

- writes like function definition
- writes declaration with `function*`
- in ES6 method shorthand the `*` goes before the name
- `yield` operator in body specifies next value in iteration sequence, in the value-done object `value` is the evaluated expression and `done` is `false`
- `return` operator in body specifies last value in iteration sequence, in the value-done object `value` is the evaluated expression and `done` is `true`
- `throw` operator in body (uncaught) throws value on the next iteration, closes iterator as well
- can use `try...catch...finally`, `finally` runs when iterator is closed
- looks like function that pauses and resumes where it left off, returning multiple values through pauses, accepting multiple values through resumations, looks like can influence function while it's running

```javascript
function* gen() {
    const a = 21;
    yield a;
    const b = a * 2;
    yield b;
}

const it = gen();

console.log(it.next()); // { value: 21, done: false }
console.log(it.next()); // { value: 42, done: false }
console.log(it.next()); // { value: undefined, done: true }
```

- beware: when called doesn't run the body, unlike a normal function ❗️
- beware: when continued doesn't run body to completion, unlike a normal function ❗️
- `yield*` delegates to another iterable iterator, e.g. built-in iterable, generator function, etc.

```javascript
function* foo() {
    yield 3;
    yield 4;
}

function* gen() {
    yield* [1, 2];
    yield* foo();
    yield 5;
}

const it = gen();

console.log(it.next()); // { value: 1, done: false }
console.log(it.next()); // { value: 2, done: false }
console.log(it.next()); // { value: 3, done: false }
console.log(it.next()); // { value: 4, done: false }
console.log(it.next()); // { value: 5, done: false }
console.log(it.next()); // { value: undefined, done: true }
```

### Consumer

- iterator has `.next()`, `.return()`, `.throw()` methods
- methods accept arguments, as if continues, returns, throws at the place of the current `yield`
- beware: arguments to first `.next()` are silently ignored, since only starts execution of generator until first `yield`, then second `.next()` replaces first `yield`, i.e. `.next()` call and the `yield` expression it replaces are offset by one ⚠️

### Examples

- can think of generator as function that magically transforms its body, returns an object with a `.next()` method that contains a big switch statement, a case for each code block until the next `yield`, keeps a counter and the shared state in the closure, etc.
- beware: analogy is simplified, much more complex to correctly identify shared state, handle input values into methods, etc. ❗️

```javascript
function generator() {
  /* shared state */

  let i = 0;

  return {
    next() {
      switch (i++) {
        case 0:
          /* computation of "A" */
          return { value: "A", done: false };

        case 1:
          /* computation of "B" */
          return { value: "B", done: true };

        default:
          return { value: undefined, done: true };
      }
    },
    [Symbol.iterator]() { return generator(); }
  };
}

function* generator() {
    /* shared state */

    /* computation of "A" */
    yield "A";

    /* computation of "B" */
    return "B";
}
```

- `return yield` makes one more iteration that `return`

```javascript
function* gen() {
    return yield 1;
    // can think of
    // const _tmp = yield 1;
    // return _tmp;
}

const it = gen();

console.log(it.next("a")); // { value: 1, done: false }
console.log(it.next("b")); // { value: "b", done: true }
console.log(it.next("c")); // { value: undefined, done: true }
```

```javascript
function* gen() {
    return 1;
}

const it = gen();

console.log(it.next("a")); // { value: 1, done: true }
console.log(it.next("b")); // { value: undefined, done: true }
console.log(it.next("c")); // { value: undefined, done: true }
```

- simpler syntax to create an iterable iterator

```javascript
function* gen() {
    let id = 0;

    while (true) {
        yield id;
        id += 1;
    }
}

const counter = gen();

console.log(counter.next()); // { value: 1, done: false }
console.log(counter.next()); // { value: 2, done: false }
// ...
```

```javascript
function* gen() {
    let n1 = 0;
    let n2 = 1;

    while (true) {
        yield n1;
        n1 = n2;
        n2 = current + n2;
    }
}

const fibonacci = gen();

console.log(fibonacci.next()); // { value: 0, done: false }
console.log(fibonacci.next()); // { value: 1, done: false }
// ...
```

- remember the `return`ed value doesn't show up in built-in iterations

```javascript
// range [start, end)
function* gen(start = 0, end = Infinity, step = 1) {
    let i = start;

    while (i < end) {
        yield i
        i += step;
    }

    return i;
}

const it = gen(0, 3);

console.log(it.next()); // { value: 0, done: false }
console.log(it.next()); // { value: 1, done: false }
console.log(it.next()); // { value: 2, done: false }
console.log(it.next()); // { value: 3, done: true }
console.log(it.next()); // { value: undefined, done: true }

for (const value of gen(0, 3)) {
  console.log(value); // 0 1 2
}

console.log(...gen(0, 3)); // 0 1 2
```

- can make object iterable

```javascript
// Note: properties added to the object in the for...of loop are not visited, since the iterable is created (i.e. the generator executed) at the beginning of the loop
Object.prototype[Symbol.iterator] = function*() {
    for (const [key, value] of Object.entries(this)) {
        yield { key, value };
    }
};

const o = { name: "Peter", age: 42 };

for (const v of o) {
    console.log(v);
}
// { key: 'name', value: 'Peter' }
// { key: 'age', value: 42 }
```



## Asynchronous Iterators and Generators

- like iterators and generators but async (with ES2018)
- async iterator like iterator but `.next()` returns promise of value-done object
- async iterable like iterable but with symbol `Symbol.asyncIterator`
- `for...await...of` statement like `for...of` statement but for async iterable, awaits promise of async iterator's `.next()` method, allowed only where async await is allowed
- async generator like generator, but returns async iterable iterator (where `.return()` and `.throw()` also return promise of value-done object), `yield*` also allows async iterables, allows async await
- beware: don't confuse with async await, isn't the same ❗️



## Resources

- MDN - as usual