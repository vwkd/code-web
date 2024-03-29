---
title: Promises
author: vwkd
index: 10.2
tags:
  - languages
  - javascript
---

- special object that manages callbacks (with ES6)
- can think of like a box that will contain a value in the future, will notify callback when that value arrives
- gets promise object from async function instead of giving callback
- input and output are decoupled again 🎉
- fixes problems of CPS



## Consumption

- promise object has two internals properties
  - `[[PromiseState]]`: can be `pending`, `fulfilled`, or `rejected`
  - `[[PromiseValue]]`: can be any value
- while async operation is running, promise has `pending` state and `undefined` value
- after async operation finished, promise settles to `fulfilled` or `rejected` state with corresponding success or error value
- `.then(onFulfilled, onRejected)`: registers handler on promise object
- promise asynchronously calls `onFulfilled`/`onRejected` handler depending on its state after it settled with its value as argument
- gives back guarantees, e.g. only one handler is called not both, handler is only called once, handler is passed only single argument, handler is always called asynchronously 🎉
- can register multiple handlers to same promise, get called in order of attachement
- beware: one handler can still get called more than once if registers more than once ❗️
- can register handler later after promise settled, value isn't lost, handler is called immediately (still asynchronously!)
- beware: if doesn't register error handler errors are swallowed, since promise can't know if doesn't attach one later, runtimes usually log an unhandled rejection, e.g. "uncaught exception", some even die completely, e.g. Deno, Node ❗️
- beware: don't attach handler later, makes code easier to reason about ❗️
- `.catch(onRejection)`: shorthand for `.then(undefined, onRejected)`
- `.finally(onFinally)`: registers handler on promise object that's always called once settled without an argument, e.g. for cleanup (with ES2018)
- method returns itself a promise
- if handler throws, then wraps thrown value in `Promise.rejected()`
- if handler returns, then wraps return value in `Promise.resolve()`, except `.finally()` forwards the previous promise
- beware: same for both `onFulfilled` and `onRejected` handlers, promise returned by method doesn't depend on type of handler that is called, just if handler returns / throws ❗️
- beware: remember in JS a function always returns, either explicitly or implicitly `undefined`, see Functions ❗️
- gives back return and throw 🎉
- allows chaining of subsequent async calls
- subsequential async calls are linear, not nested 🎉
- beware: in handler needs to return subsequent async call, otherwise won't wait for promise to settle ❗️

```javascript
// ❌
somePromise().then(val => {
  anotherPromise();
})

// ✅
somePromise().then(val => {
  return anotherPromise();
})
```

- if handler is not a function, then method registers default onFulfilled / onRejected handler which just returns / rethrows
- values / errors propagate through chain until there's a handler to handle it, can think of as "skipping" incapable handlers
- gives back call stack 🎉
- should always end promise chain with a single error handler
- beware: that last error handler shoud never throw, otherwise gets unhandled rejection ⚠️
- beware: handlers are run async, can't ever merge code paths with sync code outside, 
- can think of a promise like an event target that emits only a single event, caches it for later listeners, listeners can decide which two types of events to listen for, and listeners can chain themselves
- beware: event analogy breaks down under the hood, because uses microtask queue instead of macrotask queue ❗️



## Construction

- use `Promise` constructor
- takes executor function as single argument
- executor is called with two arguments, e.g. call `res`, `rej`
- if executor calls `res`, the promise is resolved with the first value passed as argument
  - if value is a thenable or promise, then unwraps it and creates a promise adopting the same state and value
  - if value is a non-promise, then creates fulfilled promise
- resolve means settle (fulfill or reject) or match the state of another promise (which might still be pending, i.e. not settled)
- beware: for a non-thenable "resolve" means "fulfill", but for a non-promise thenable "resolve" can mean "reject" as well, therefore "settle" is actually correct ❗️
is computing the state, while settle (fulfill or reject) is changing the state ⚠️
- if executor calls `rej`, the promise is rejected with the first value passed as argument
- any further calls to `res` or `rej` are silently ignored, promise settles only once
- any further arguments to `res` or `rej` are silently ignored, promise settles only with single value
- return value of executor itself is ignored
- if throws in executor, the promise is rejected with the thrown value, even if no `res` or `rej` was called
- beware: undefined variables throw when accessed, e.g. `res(something)` where `something` is `undefined` ❗️
- beware: don't confuse construction with consumption, construction is synchronous, consumption is asynchronous ❗️

### `Promise.resolve()`

- creates promise that is resolved with the value
  - if value is a promise itself, then just returns that promise
  - if value is a thenable, then unwraps it and creates a promise adopting the same state and value
  - if value is a non-promise, then creates fulfilled promise
- can use to wrap value in a promise
- can use to make an untrusted thenable safe, e.g. calls both handlers, calls multiple times, passes multiple arguments, calls synchronously, etc.

```javascript
const thenable = {
	then: function(onFulfilled, onRejected) {
		onFulfilled("success", "foo");
		onFulfilled("success2", "foo2");
		onRejected("error", "bar");
	}
};

thenable.then(console.log, console.log);
console.log("AFTER");
// success foo
// success2 foo2
// error bar
// AFTER

const p = Promise.resolve(thenable);

p.then(console.log, console.log);
console.log("AFTER");
// AFTER
// success
```

- `Promise.resolve(..)` isn't the same as `new Promise((res, _) => {res(..)})`, since for promise itself just returns it instead of unwrapping it to create a new promise ❗️

```javascript
const p1 = Promise.resolve("A");
const p2 = Promise.resolve(p1);
const p3 = Promise.resolve("B");

p2.then(console.log);
p3.then(console.log);
// A
// B
```

```javascript
const p1 = Promise.resolve("A");
const p2 = new Promise(function (res, _) {
  res(p1);
});
const p3 = Promise.resolve("B");

p2.then(console.log);
p3.then(console.log);
// B
// A
// --> p2 takes two ticks instead of one!
```

- beware: order how handlers of different promises are called isn't necessarily as expected, see example above ❗️

### `Promise.reject()`

- creates promise that is rejected with the value
- beware: `Promise.reject(..)` is the same as `new Promise((_, rej) => {rej(..)})` ❗️

```javascript
const p1 = Promise.reject("A");
const p2 = Promise.reject(p1);
const p3 = Promise.reject("B");

p1.then(undefined, () => {}); // just to prevent an unhandled rejection from p1
p2.then(undefined, console.log);
p3.then(undefined, console.log);
// Promise {<fulfilled>: "A"}
// B
```

```javascript
const p1 = Promise.reject("A");
const p2 = new Promise(function (_, rej) {
  rej(p1);
});
const p3 = Promise.reject("B");

p1.then(undefined, () => {}); // just to prevent an unhandled rejection from p1
p2.then(undefined, console.log);
p3.then(undefined, console.log);
// Promise {<fulfilled>: "A"}
// B
```



## Helper

### `Promise.all(iterable)`

- creates promise that waits for all, but short-circuits on error
- fulfills if all fulfill, value is array of values of all promises
  - order of values is same as in iterable no matter which fulfilled first
  - non-promise values in iterable are directly passed into result array
- rejects with error of first that rejects
  - others continue running
  - results of others are ignored no matter if fulfill or reject
- if iterable is empty fulfills
- allows to run operations "in parallel"

### `Promise.allSettled(iterable)`

- creates promise that waits for all, but doesn't short-circuit on error (with ES2020)
- fulfills if all settle, value is array of status-value/reason objects of all promises
- beware: never rejects ❗️
- beware: needs to handle object with different properties depending on state ❗️
- order of values is same as in iterable no matter which fulfill / reject first
- non-promise values in iterable are directly passed into result array
- if iterable is empty fulfills
- allows to run operations "in parallel"

### `Promise.race(iterable)`

- creates promise that waits until first settles
- fulfills / rejects as soon as first fulfills / rejects with value
- other promises continue running but results get ignored
- beware: if iterable is empty will be pending forever, bug in spec ⚠️
- allows to emulate abort by racing with a promise of known delay, but doesn't actually abort the other

### `Promise.any(iterable)`

- creates promise that waits until first fulfills (with ES2021)
- fulfills as soon as first fulfills with value
- rejects if all reject, value is error object (not array of values)
- if iterable is empty rejects
- other promises continue running but results get ignored



## Problems

- doesn't get rid of callbacks
- handler in promise chain can't access state from previous handlers due to separate function scope, needs to pass value along explicitly
- no `try...catch`
- can't abort, promise can stay unresolved forever
- can't protect from unhandled rejection in last handler in promise chain



## Resources

- [Domenic Denicola - States and Fates](https://github.com/domenic/promises-unwrapping/blob/master/docs/states-and-fates.md)