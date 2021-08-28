---
title: CPS
author: vwkd
index: 10.1
tags:
  - javascript
---
# Continuation Pasing Style (CPS)



## Introduction

- passes handler as callback argument
- by convention first argument of callback is for errors

```javascript
function callback(error, data) {
  if (error) {
    // handle error
  }
  // handle data
}

asyncOperation(..., callback);
```



## Problems

- misuses callback as control flow, conflates input and output, gives input to handle output
- subsequential async calls are nested, "callback hell"
- no guarantees how callback is called, e.g. if called, how often called, with what arguments called, if called synchronously or asynchronously, etc.
- can't return, because would yield back into API
- can't throw errors, because would yield back into API
- needs to handle errors, can't ignore, not automatically passed up the call stack until someone handles it
- looses call stack