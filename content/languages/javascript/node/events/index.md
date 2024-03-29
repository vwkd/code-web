---
title: Events
author: vwkd
index: 3
tags:
  - languages
  - javascript
  - node
---

- events work differently than in browser
- events are triggered manually, no browser window that magically triggers events through interaction
- there are no built-in events, everything is created manually
- there are no event objects, can pass arguments to EH when triggering event, event is just event name
- event targets are called event emitters, more accurate name
- there is no event flow



## Event emitters

- EM objects are instances of `EventEmitter`
- `eventEmitter.attachListener()`/`on()`: adds EH to EM for event name, returns EM so calls can be chained
- `eventEmitter.removeListener()`/`off()`: removes EH from EM for event name, returns EM so calls can be chained
- `eventEmitter.once()`: adds one-time EH to EM for event name, returns EM so calls can be chained, i.e. like `on()` for an EH which calls `off()` inside itself
- `eventEmitter.emit()`: calls EHs for event name _synchronously_, in order of registration, passing arguments to each, returns boolean if any EHs are attached

```javascript
const EventEmitter = require("events");

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on("something", () => {
    console.log("Reacting to event...");
});

myEmitter.on("error", err => {
    console.error("Whoops, an error occurred!", err);
});

myEmitter.emit("something");
// Reacting to event...

myEmitter.emit("error", new Error("Illegal operation."));
// Whoops, an error occurred! Error: Illegal operation.
```



## Event handlers

- EHs for same event are called _sync_ and in order of attachment, must be non-async functions ❗️
- `this` inside EH refers to EM which EH is registered on
- return value of EH is ignored
- can attach at most `10` EHs for given event by default, more will output a trace warning to `stderr`, can change using `eventEmitter.setMaxListeners()`
- if EH is attached multiple times for same event and EM, it is called multiple times, needs to be removed multiple times as well ❗️
- on `"error"` event, if no EH is attached, the first argument is thrown, a stack trace is printed, and the Node.js process exits, therefore always attach an error handler ❗️
- removing EHs won't stop calls for already emitted events



## Resources