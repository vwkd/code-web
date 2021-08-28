---
title: Async await
author: vwkd
index: 10.3
tags:
  - javascript
---

- sugar syntax for combination of generators and promises (with ES2017)
- solves problems of Promises 🎉



## Asynchronous function

- generator function that yields promises (pauses)
- outside pulls promise out, waits for promise to resolve, pushes result back in as value or error
- iterator is iterated asynchronously since promise handler runs asynchronously
- wrap the supposed promise in `Promise.resolve()` to make sure it's a promise and always asynchronous

```javascript
function* gen() {
    const a = 21;
    const val = yield somePromise(); // or yield* if another iterator
    const b = a * val;
    return b;
}

const it = gen();

const {value, _} = it.next();

Promise.resolve(value)
  .then(val => it.next(val), err => it.throw(err))
  .then(.., ..)
```

- generator looks like function that pauses and resumes where it left off *asynchronously*, waiting for the yielded promise to settle, then continuing with its value if it fulfilled or throwing the error if it rejected
- the code block after a `yield` effectively becomes the handler of the promise it yielded since it's passed the result
- can think of `yield` attaching that code as a handler when reached
- multiple handler look like one code block, no promise chain 🎉
- handler can access state from previous handlers since generator keeps shared state 🎉
- gets back `try...catch` 🎉
- can safely `yield` (and `return` and `throw`) any value, since is wrapped as a promise with `Promise.resolve()`
- last promise settles with return value or uncaught error thrown
- beware: `return`ing a promise instead of `return yield`ing it doesn't wait for it to settle, little difference when it fulfills, but if it rejects can't catch it in `try...catch` inside ❗️



## Driver

- helper function that automates iteration
- hides the plumbing away
- works for generator which yields any number of promises
- returns last promise of generator

```javascript
function driver(gen, ...args) {
  const it = gen(...args);
  return new Promise((res, rej) => {

    (function pushBackIn(val, err) {
      try {
        // in first recursion is `it.next()` since both `val` and `err` are `undefined`
        const { value, done } = err ? it.throw(err) : it.next(val);

        if (done) {
          res(value);
        } else {
          Promise.resolve(value).then(val => pushBackIn(val, undefined), err => pushBackIn(undefined, err));
        }
      } catch (err) {
        rej(err);
      }
    })();

  });
}

driver(gen).then(.., ..)
```



## Async await

- sugar that abstracts away driver of generator
- uses `async function` instead of `function*` and `await` instead of `yield`(`*`)
- calls async function directly instead of driver with generator
- beware: can use async function as arrow function, unlike generator yet as of 2021 ❗️

```javascript
async function func() {
    const a = 21;
    const val = await somePromise();
    const b = a * val;
    return b;
}

func().then(.., ..);
```



## Top-level await

- await in top-level of module
- module is treated as if it were within an async function
- beware: takes at least one trip through the microtask queue before it's run ❗️



## Resources

- [Jake Archibald - await vs return vs return await](https://jakearchibald.com/2017/await-vs-return-vs-return-await/)