---
title: Keyed Collections
author: vwkd
index: 6
tags:
  - languages
  - javascript
---

## `Map` object type

```javascript
const mp = new Map([[key1, value1], [key2, value2]]);

const mp = new Map();
mp.set(key1, value1);

mp.get(key1)
```

- collection of key-value pairs, a better object 🎉
- any data type as key, not only String or Symbol 🎉
- as keys `NaN` is considered equal to itself, `+0` and `-0` are considered equal to each other 🎉
- iterable with value `[key, value]`, e.g. can use for...of instead of for...in, no prototype leakage anymore 🎉
- iteration order is insertion order of keys (for objects is only since ES6) 🎉
- has `size` property 🎉
- has iteration methods `Map.prototype.keys()`, `Map.prototype.values()`, `Map.prototype.forEach()` 🎉
- lookup operations are faster 🎉
- beware: don't use dot / bracket notation, since properties are set on map as object properties instead of map properties ⚠️



## `WeakMap` object type

- `Map` object, but only objects as keys
- keys are weekly referenced, i.e. garbage collection isn't prevented incase there's no other reference to the object
- good to prevent memory leaking
- not enumerable, because garbage collection is not determined



## `Set` object type

```javascript
const st = new Set([1, 2, 3, 3, 3]);

const st = new Set();
st.add([1, 2, 3, 3, 3]);
```

- collection of values, like array
- values must be unique, duplicates get discarded, i.e. can make array unique by converting to set and back ❗️
- no access to value at index, because no actionable order due to duplicate filter
- can delete elements by value since uniquely identifiable, in array needs to splice by index
- as values `NaN` is considered equal to itself, `+0` and `-0` are considered equal to each other 🎉
- has `size` property 🎉
- lookup operations are faster, e.g. `Set.prototype.has()` is faster than `Array.prototype.indexOf()` 🎉
- beware: don't use dot / bracket notation, since properties are set on set as object properties instead of set properties ⚠️



## `WeakSet` object type

- `Set` object, but "weak", like what `WeakMap` is to `Map`



## Resources

- MDN - as usual