---
title: Arrays
author: vwkd
index: 5
tags:
  - languages
  - javascript
---

- ordered set of values, indexed collection
- special objects with dynamic length property, constructor is `Array`
- indexes are just integer property names, starting from `0`, can add other properties like with any object
- can access integer property names only using bracket notation since integers are not valid identifiers, e.g. `arr[5]`
- length property returns largest integer property plus 1, i.e. other properties don't count against length
- values can be of any data type, arrays grow / shrink dynamically without bounds since just object, i.e. not "out of bound" error
- beware: arrays are _not_ linear allocations of memory like in other languages, objects that are made to look like it, i.e. not fast at all ❗️
- object is "array-like" when it has a length property and indexed elements, it lacks all methods of `Array.prototype` like `forEach()`, `push()`, etc.



## Creating arrays

- array literal: `[element0, element1]`
- array constructor: `new Array(element0, element1)`, if argument single integer creates array with that length and all values `undefined`
- see `Array.of()` and `Array.from()` later



## Array elements

- doesn't need to assign to indexes in ascending order, can just assign to any property like object

```javascript
const arr = ["Hello", "World"];
arr[42] = "Illuminati";
```

- any non-integer element becomes a typical property name as string

```javascript
arr[3.5] = 1010; // creates property name "3.5"
console.log(arr["3.5"]) // 1010
```

- length property returns last index plus 1, computed internally

```javascript
console.log(arr.length); // 43
```

- increasing the length property does _not_ expand the array, just increases length property  
  beware: accessing non-existing elements returns `undefined` even though they do not exist at all, they are not just `undefined` ❗️
- decreasing the length property truncates array, all higher indexes are deleted
- using delete on an array item doesn't update the `length` property of the array, use `Array.prototype.splice()` instead



## Array iteration

### for loop 👎

```javascript
for (let i = 0; i < arr.length; i++) {
  statements;
}
```

- loops over all indices of an array
- does not skip non-existing elements, since`i` increases until length property ❗️

### for...in loop 👎

- see Basics

### for...of loop 👍

```javascript
for (const item of arr) {
  statements;
}
```

- loops over all indexed elements of an array
- does not skip non-existing elements, i.e. like a for loop ❗️

### `Array.prototype.forEach()` 👎

```javascript
arr.forEach((currentElement, i) => {...});
```

- for each element calls a function with element, index and optional parameters as argument
- skips non-existing elements
- ignores return value of callback function
- returns itself `undefined`



## `Array` object type

### Properties

- `Array.from()`: creates new array from iterable or array-like object
- `Array.isArray()`: checks if argument is array, fix for broken `typeof` operator
- `Array.of()`: creates new array from arguments

### Prototype properties

- `Array.prototype.concat()`: joins array(s) to back of existing
- `Array.prototype.join()`: joins elements of array to string using separator
- `Array.prototype.push()`: adds element(s) to end of array, returns new length
- `Array.prototype.unshift()`: adds element(s) to front of array, returns new length
- `Array.prototype.pop()`: removes last element from array, returns this element
- `Array.prototype.shift()`: removes first element from array, returns this element
- `Array.prototype.slice()`: returns section of array
- `Array.prototype.splice()`: replaces / removes section from array, returns section
- `Array.prototype.reverse()`: returns reversed array
- `Array.prototype.sort()`: returns sorted array, can specify sort function
- `Array.prototype.includes()`: returns `true` if array contains element, else `false` (with ES2016)
- `Array.prototype.indexOf()`: returns index of first matching element
- `Array.prototype.lastIndexOf()`: returns index of last matching element
- `Array.prototype.find()`: returns first element for which calling the function returned `true`
- `Array.prototype.findIndex()`: returns index of first element for which calling the function returned `true`
- `Array.prototype.forEach()`: calls a function for each element
- `Array.prototype.map()`: returns array with results from calling a function for each element
- `Array.prototype.filter()`: returns array with all elements for which calling the function returned `true`
- `Array.prototype.every()`: returns `true` if function returned `true` for each element
- `Array.prototype.some()`: returns `true` if function returned `true` for at least one element
- `Array.prototype.reduce()`: combines elements by recursively calling a function from left-to-right with previous result as argument
- `Array.prototype.rightReduce()`: reduce from right-to-left
- `Array.prototype.flat()`: returns flattened array up to the specified depth (with ES2019)



## Resources

- MDN - as usual