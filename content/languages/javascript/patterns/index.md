---
title: Patterns
author: vwkd
index: 10.4
tags:
  - javascript
---

- has async operations and handler
- can have dependent or independent ops, can have single handler or separate
- beware: here omit error handling for readability, imagine error handler as second argument to handling `.then()` and `try...catch` around `await` ❗️



## Dependend ops

- ops are dependent on each other
- run ops sequentially
- if first op fails second op never starts

|op1|op2|total duration|
|--|--|--|
|fulfills|fulfills|d1+d2|
|rejects|N/A (since never runs)|d1|
|fulfills|rejects|d1+d2|

### Handle together

- single handler

```javascript
op1()
.then(val1 => Promise.all([val1, op2(val1)])
.then([val1, val2] => {
    // ... handle both val1 and val2
})
```

```javascript
const val1 = await op1();
const val2 = await op2(val1);
// ... handle both val1 and val2
```

### Handle separately

- separate handlers

```javascript
op1()
.then(val1 => {
    // ... handle val1
    return val1;
})
.then(val1 => op2(val1))
.then(val2 => {
    // ... handle val2
    return val2;
})
```

```javascript
const val1 = await op1();
// ... handle val1
const val2 = await op2(val1);
// ... handle val2
```



## Independent ops

- ops are independent of each other
- run ops concurrently
- if doesn't fail-fast on first rejection

|op1|op2|total duration|
|--|--|--|
|fulfills|fulfills|d of longer|
|rejects|fulfills|d of longer|
|fulfills|rejects|d of longer|
|rejects|rejects|d of longer|

- if fail-fast on first rejection

|op1|op2|total duration|
|--|--|--|
|fulfills|fulfills|d of longer|
|rejects|fulfills|d1|
|fulfills|rejects|d2|
|rejects|rejects|d of shorter|

### Handle together

- single handler

```javascript
Promise.all([op1(), op2()])
.then([val1, val2] => {
    // ... handle both val1 and val2
})
```

```javascript
const [val1, val2] = await Promise.all([op1(), op2()]);
// ... handle both val1 and val2
```

- beware: don't simply `await` promises later, because if second settles before first then it has no handler attached, because its `await` hasn't been reached yet ⚠️

```javascript
const p1 = op1();
const p2 = op2();

const val1 = await p1;
const val2 = await p2;
```

### Handle separately

- separate handlers
- beware: if fail-fast on first rejection, handler of other op will run regardeless because is already registered ❗️

```javascript
op1().then(val1 => {
    // ... handle val1
})

op2().then(val2 => {
    // ... handle val2
})
```

```javascript
async function do1() {
    const val1 = await op1();
    // ... handle val1
}

async function do2() {
    const val2 = await op2();
    // ... handle val2
}

do1();
do2();
```