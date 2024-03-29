---
title: Asynchrony
author: vwkd
index: 10
tags:
  - languages
  - javascript
---

<!-- todo: finish -->
<!-- todo: read
https://www.slideshare.net/domenicdenicola/the-promised-land-in-angular
https://www.slideshare.net/domenicdenicola/callbacks-promises-and-coroutines-oh-my-the-evolution-of-asynchronicity-in-javascript

https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide
 -->

- concurrent asynchrony
- async operations are created using Promises or Web APIs
<!-- todo:
note that in pure JS (without Web APIs) only Promises can create async operations
 -->
- beware: JS operations are not only synchronous (without Web APIs), e.g. Promises are asynchronous ❗️
- beware: Web API operations aren't all asynchronous, e.g. BOM API and DOM API are synchronous ❗️



## Operations

- top-level of script is like a main operation, runs first, schedules any nested async operations
- after main operation is done, async operations run, can schedule themselves more nested async operations
- order of async operations depend on type:
  - first *all* microtasks, e.g. Promises, MutationObserver API, etc.
  - then *one* macrotask, e.g. Timeout API, Fetch API, Event API, etc.
- beware: order of nested async operations gets confusing quickly ❗️

```javascript
// running order: op_main, op2, op21, op1, op211, op11, op111

//op_main

setTimeout(() => {
  //op_1: macrotask

  setTimeout(() => {
    //op_11: macrotask

    Promise.resolve().then(() => {
      //op_111: microtask
      console.log("op_111");
    })

    console.log("op_11");
  }, 0)

  console.log("op_1");
}, 0)

Promise.resolve().then(() => {
  //op_2: microtask

  Promise.resolve().then(() => {
    //op_21: microtask

    setTimeout(() => {
      //op_211: macrotask
      console.log("op_211");
    }, 0)

    console.log("op_21");
  })

  console.log("op_2");
})

console.log("op_main");
```



## Code

<!-- todo: handler function receives result from Web API (if any) as argument -->
<!-- ??? handler is itself the operation -->
calls asynchronous function
passes handler function that runs with results of async operation
- see CPS, Promises, Async await
- needs to write "parallel" code in a linear text file, like multiple dimensions in single dimension
- beware: order of code isn't order of execution anymore, can't read code top-to-bottom to know what is run when ⚠️
- beware: operations are separate, can't merge code paths again after separated, async code is called only after sync code is done ⚠

- beware: same handler may be called multiple times, e.g. Event API ❗️



## Implementation

- RE runs JS on a single thread, "main thread"
- Web browser uses same main thread to create DOM, e.g. parse HTML, style calc, layout, paint, etc.

- many Web APIs run on separate thread, don't block main thread where JS + DOM runs
some don't need separate thread, e.g. Timeout API
just puts into queue, only needs threads for actual tasks, e.g. network, storage, etc.
- beware: don't confuse Web APIs with handlers, all JS runs on main thread, only actual work of Web API is on separate thread, e.g. fetch, I/O, Web Workers, etc.
- depends on implementation if threads in same or different processes, e.g. different processes in Chrome

### Event Loop

- loop in RE that selects operation(s) for execution
- one iteration is called a "tick"
- like a manager that decides what operation is run when
- if no operation runs, event loop takes a handler from queue and executes it
- beware: only one handler can run at a time ❗️
- beware: handler is always run to completion uninterrupted, e.g. no flashing screen while changing the DOM ❗️
- Web browser also created DOM at the end of a tick if necessary
- beware: a long running handler blocks the thread, because the event loop can't continue, e.g. in browser UI freezes ⚠️
- split long running handler into smaller async operations, if purely computational use seperate thread through Web Workers

### Task Queues

- scheduling an async operation, puts the handler into a queue
?? if Web API then only after RE did some work, often in separate thread
  gets results from other threads back onto main thread
- can think of as mailbox that collects mail to open later

- multiple queues, depend on runtime with Web APIs, differ in when and how they are run
  for a tick, from some queues only a single handler is run, from some queues all handlers are run (no new ones), from some all handlers are run (even new ones)
- order of queues for one tick
  - *all existing and new* microtasks, e.g. Promises, MutationObserver API, etc.
  - *one* macrotask, e.g. Timeout API, Fetch API, Event API, etc.
  - *all existing* requestAnimationFrame if rendering
  - the render pipeline if needed, adapts intelligently to device, e.g. every ~16 ms on 60 FPS screen, not if in background, etc.
  - own queues: `IndexedDB`, `requestAnimationFrame`, etc.
- beware: implementation can still vary, e.g. skip Timeout API macrotask for rendering mouse clicks, etc. ❗️

- microtask run even mid macrotask if no operation is running anymore, e.g. between two listener for single
- beware: event initiated through JS can behave differently than through user, because through JS is scheduled synchronously, and microtasks can't run mid macrotask anymore because the main operation is still running ⚠️

- beware: a long queue delays execution of handlers, because needs to wait until all handlers before are run, e.g. `setTimeout(() => {}, x)` may not be in x milliseconds ⚠️
- beware: an infinite Microtask queue blocks the thread, because the event loop can't continue ⚠️

?? how does Web API on macrotask queue can return a promise on the microtask queue ??
- promisifying non-promise Web API goes through macrotask queue and then through microtask queue during same tick, but shouldn't be a problem since microtask queue is always empty before a macrotask runs

```javascript
function timeout(delay, args) {
  return new Promise((res, _) => {
    setTimeout(res, delay, args)
  })
}

timeout(1000).then(() => console.log("Hello World!"))
```

### `queueMicrotask()`

- schedules a microtask
- executed in same tick



## Non-determinism

- order of async operations from Web APIs is non-deterministic
can't guarantee order of asynchronous operations from Web APIs
- since handler is put into queue only after Web API came back, and Web API may depend on external factors, e.g. IO, network, etc.
- beware: even for simple Web APIs shouldn't rely on order, e.g. two subsequent `setTimeout(..., 0)` calls may or may not be in order ❗️

```javascript
// logs a b or b a, no guarantees
fetch("a.com").then(() => {console.log("a")});
fetch("b.com").then(() => {console.log("b")});
```

### Race conditions

- non-deterministic async operations that access and modify the same shared state, e.g. global variables, DOM, etc.
- result changes for different executions
- since handlers share same state because in same scope
- common source of bugs

```javascript
// logs 6 or 4, no guarantees
let a = 1;
Promise.all([
  fetch("a.com").then(() => {a = a + 2;}),
  fetch("b.com").then(() => {a = a * 2;})
  ]).then(() => {console.log(a)});
```



## Resources

- [Philip Roberts - What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [Jake Archibald - In The Loop](https://vimeo.com/254947206)
- [Jake Archibald - Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules)
- [Kyle Simpson - You Don't Know JS: Async & Performance](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed/async%20%26%20performance)
- [Domenic Denicola - Async Frontiers in JavaScript](https://player.vimeo.com/video/132786072)