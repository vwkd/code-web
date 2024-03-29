---
title: Modules
author: vwkd
index: 11
tags:
  - languages
  - javascript
---

- encapsulation of data with controlled visibility and access 
- used to organise data, bundle related code
- expose only necessary interfaces, keep implementation details private, principle of least exposure
- avoids name collisions and allows for future refactoring



## Classic modules

- use function and closure to create modules from objects and hide state in closure, e.g. in IIFE

```javascript
// without closure
const p = {
    name: "Peter",
    sayHi(greeting) {console.log(`${greeting}, ${this.name}!`);}
}

p.sayHi("Hello"); // Hello, Peter!
p.name; // "Peter" 👎

// with closure
const p = (function() {
    name = "Peter";
    function sayHi(greeting) { console.log(`${greeting}, ${name}!`);};

    return { sayHi };
})();

p.sayHi("Hello"); // Hello, Peter!
p.name; // undefined 👍
```



## ES Modules

- standardise modules in JavaScript (with ES6)
- one module per file
- modules have strict mode enabled by default 🎉
- modules have their own scope, i.e. variable names in different files don't conflict, no need to wrap in IIFE anymore 🎉
- when loading, modules are not parsed, only searched for `import` statements, then loads all dependencies, only after all are loaded actually runs, i.e. module specifiers can not contain variables since not parsed ❗️
- imported modules are run so can use them, i.e. any side effects are executed as well
- deepest modules are run first, in order of import statements



### Export

- can export any identifiers in top-level, e.g. variable, function, object, class, etc.

#### Named export

- add `export` keyword before expression
- arbitrarily many possible

```javascript
// lib.js

export const x = 42;

export function sayHi() {}

export class Person {}
```

- can bundle in modules object
- modules object can come before declaration, e.g. on top of file

```javascript
// lib.js
export {x, sayHi, Person};

const x = 42;

function sayHi() {}

class Person {}
```

- can rename exports in module object by appending `as <name>`

```javascript
// lib.js

const x = 42;

function sayHi() {}

class Person {}

export {x as y, sayHi as greet, Person};
```

- can export something multiple times, e.g. useful for version and most recent

```javascript
// lib.js

function v1() {}

function v2() {}

function v3() {}

export { v1 as version1, v2 as version2, v3 as version3, v3 as latest};
```

#### Default export

- add `export default` keyword before expression
- only one per module
- can combine with named exports

```javascript
// lib.js

export const x = 42;

export function sayHi() {}

export default class Person {}
```



### Import

- can import any modules in top-level
- import must happen before use
- duplicate imports are ignored, entity is instantiated only on first import
- module specifier: string that specifies the location of the module, file path, as of March 2020 can not be bare and need to have file extension
- module specifier can not contain variables since modules are not parsed for loading
- module specifier depends on environment, i.e. in browser needs to be valid URL with extension️, e.g. relative `./path/to/file.js` or absolute `/path/to/file.js` ❗

#### Named import

- add `import <object destructuring> from <module specifier>`
- like object destructuring, think of module as export object

```javascript
// main.js

import { x, sayHi, Person } from './lib.js'
```

- can rename imports by appending `as <name>`

```javascript
// main.js

import { x as y, sayHi as greet, Person } from './lib.js'
```

- can import as module object, i.e. exports are properties of single object

```javascript
// main.js

import * as Utils from './lib.js'

// Utils.x, Utils.sayHi, Utils.Person
```

#### Default import

- can import using alias

```javascript
// main.js

import {default as Person} from './lib.js';
```

- or use shorthand

```javascript
// main.js

import Person from './lib.js';
```

- can use together with named imports, default import must be declared first

```javascript
// main.js

import Person, { x, sayHi } from '/lib.js';

import Person, * as Utils from '/lib.js';
```

#### Aggregating modules

- can re-export imported modules, e.g. used when exporting multiple APIs from central file

```javascript
// main.js

import ... from './lib.js';
export ...;
```

- or use shorthand
- beware: shorthand does not import module into current file, i.e. can not use in current file, need to use long version instead ❗️

```javascript
// main.js

export ... from './lib.js'
```

#### Unnamed import

- can specify no name, can not use module but code is still run, e.g. used if running module code creates side-effects

```javascript
// main.js

import './lib.js'
```

#### Dynamic imports
 
- use `import()` function in code flow to dynamically import
- returns promise that need to be handled

### Notes

- beware: circularly dependent modules, might use something in code that wasn't yet exported
- solution: don't create circular dependencies, create acyclic tree structure instead, e.g. export all constants from a single `constants.js`, not from `main.js` which wants to import modules that depend on those constants

```javascript
// file1.js

import sayHi from './file2';

sayHi(); // "Hello "

export const firstName = "Peter";
export const lastName = "Griffin";
```

```javascript
// file2.js

import { firstName } from './file1';

function sayHi() {
    console.log(`Hello ${firstName}!`);
}

export default sayHi;
```



## Resources

- [MDN - import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [MDN - export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [V8 - JavaScript modules](https://v8.dev/features/modules)