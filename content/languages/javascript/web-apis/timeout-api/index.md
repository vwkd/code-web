---
title: Timeout API
author: vwkd
index: 5
tags:
  - web-apis
---
# Timeout API



## Introduction

- API for delays
- for animation use rAF instead
- runs in (almost) every tick, much more often than screen can refresh
- runs in background even if tab is not visible
- beware: inefficient ❗️
- beware: delay is only the minimum delay, needs to wait until event loop has run all tasks that are already in macrotask queue ❗️



## `setTimeout()`

`WindowOrWorkerGlobalScope.setTimeout(callback, delay)`

- executes callback once after a delay in ms
- returns unique identifier, integer
- if timer has not yet elapsed can cancel using `clearTimeout(timeoutID)` (or `clearInterval(timeoutID)` since `setInterval()` shares the same ID pool)  
  beware: an invalid ID does not throw an error ❗️
- `this` inside callback will be back to default (global object / undefined in non- / strict mode) since callback is passed as argument to another function, can preserve it by wrapping _value of callback_ in anonymous function❗️

```javascript
setTimeout(() => {callback()}, delay)
```

- use recursive `setTimeout` for constant delay between previous call end and next call start
- use `setInterval` for constant delay between consecutive call starts
- nested `setTimeout` calls are throttled by browser, in foreground maximal every 4 ms, in background maximal every 1s


## `setInterval()`

`WindowOrWorkerGlobalScope.setInterval(callback, delay)`

- executes callback repeatedly every delay in ms, simple loop
- returns unique identifier, integer
- if timer has not yet elapsed can cancel using `clearInterval(timeoutID)` (or `clearTimeout(timeoutID)` since `setTimeout()` shares the same ID pool)  
  beware: an invalid ID does not throw an error ❗️
- `this` inside callback will be back to default (global object / undefined in non- / strict mode) since callback is passed as argument to another function, can preserve it by wrapping _value of callback_ in anonymous function❗️

```javascript
setInterval(() => {callback()}, delay)
```

- use `setInterval` for constant delay between consecutive call starts
- use recursive `setTimeout` for constant delay between previous call end and next call start
- beware: interval does not adjust to how long code takes to run, i.e. if code runs longer than delay, next call is run even before previous returned ❗️
- `setInterval` calls are throttled by browser, in foreground maximal every 4 ms, in background maximal every 1s



## Resources

- MDN - as usual
- The Modern Javascript Tutorial - as usual