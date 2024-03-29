---
title: RequestAnimationFrame API
author: vwkd
index: 6
tags:
  - languages
  - javascript
  - web-apis
---

- API for animations
- use `setInterval()` / recursive `setTimeout()` if needs specific framerate
- runs in same tick when browser schedules repaint right before (not right after)
- rAF handler scheduled in a Event API handler is executed in the same frame
- rAF handler scheduled in a rAF API handler is executed in the next frame
- doesn't run in background if tab is not visible, since browser doesn't schedule repaint
- can use to debounce frequent tasks, e.g. mouse move
- efficient since browser adapts repaint to device, e.g. frame rate not higher than max screen refresh rate, no repaint if page is not visible, etc.



## `requestAnimationFrame()`

`window.requestAnimationFrame(callback)`

- executes callback once together with next repaint of screen, no custom delay
- returns unique identifier, integer
- if browser has not yet repainted, can cancel using `window.cancelAnimationFrame(requestID)`
- callback is passed number of milliseconds since time origin as argument, is usually time at which browser context was created
- not available in Web Workers, unlike `setTimeout`/`Interval()`
- to make interval needs to call recursively

```javascript
let requestID;

function draw() {
  // your code here
  requestID = requestAnimationFrame(draw);
}

// can stop animation with cancelAnimationFrame(requestID) or build logic in recursion to not call itself anymore
```