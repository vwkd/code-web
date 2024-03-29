---
title: Objects
author: vwkd
index: 3
tags:
  - languages
  - javascript
---

- collection of key-value pairs, called "properties", properties are like variables, object is like a container for multiple variables
- every data type in JS is either an object, e.g. arrays, functions, objects, or behaves like an object, e.g. strings, numbers, booleans  
  -> needs to understand objects to understand JS
- inheritance is the distinct feature of objects in comparison to other data types
  -> can build powerful data structures



## Basics

- property names can be of data type `String` or `Symbol`
- property values can be of any data type including object, function, array, etc.  
  -> can build nested data structures of arbitrary complexity
- properties are called methods if the property value is a function
- objects are only mutable data type, properties can be modified after creation, reference stays the same, i.e. `const a = {}` and `a.name = "Peter"` is valid
- objects are addressed / passed ❓"by reference", not "by value" like primitive data types, i.e. if `a` is an object and `let b = a`, then `b` refers to the same identical object as `a`, not only a copy
- objects are compared "by reference", not "by value" like primitive data types, i.e. `let a = {}`, `let b = {}`, then `a === b` is false, because `a` and `b` are not referencing to the same object



## Object literals

### Creating object literals

```javascript
/*const car1 =*/ {make: "Honda",                             // property
                  model: "Civic",                            // property
                  year: 1998,                                // property
                  honk: function() {console.log("Hooonk");}, // method
                  beep() {console.log("Beeep");},            // method, shorthand with ES6
                  blub: alreadyExistingFunction              // method
                 }
```

- comma seperated key-value pairs, like in arrays
- if key is not valid identifier, must be enclosed in quotes, e.g. `{"1number": 42}`
- can use reserved keyword as key without quotes since ES5 (except internally used keyword `__proto__`)
- can use computed property values, i.e. variables as values

```javascript
const value1 = "Honda";
const car1 = {make: value1,
              model: "Civic",
              year: 1998};
```

- can use computed property names, i.e. variables as keys, also for ES6 method shorthand, since ES6

```javascript
const key1 = "make";
const key2 = "honk";
const car1 = {[key1]: "Honda",
              model: "Civic",
              year: 1998,
              [key2]() {console.log("Beeep");}
              };
```

- property value shorthand if property names should be the same as the variable name (with ES6)

```javascript
const make = "Honda";
const model = "Civic";
const year = 1998;
const car1 = {make,
              model,
              year};
```

- object properties are locally scoped in object, not accessible from outside, (except if object is global object)
- computed property names get converted to string if they aren't already, (uses `Object.prototype.toString()` method)
- property names must be unique, or the last will overwrite the previous, in strict mode will throw error
- can use spread operator to copy own enumerable properties from other object, see ellipsis operator

### Accessing and changing properties

- dot notation: `car1.make`, `car1.honk()`
- can only use if key is valid identifier and not computed property name, else must use bracket notation
- bracket notation: `car1[make]`, `car1[honk]()`
- can use with variable or any string in quotes as key, e.g. `obj["1number"]`
- return `undefined` if property doesn't exist
- can chain if value itself is object
- set property values by assignment, doesn't matter what variable type the object reference is, e.g. `const`

```javascript
car1.year = 1999;
car1.honk = function() {console.log("Move b****, get out da way!")};
```

- if sets property that doesn't exists a new property is created in this object  
  -> very powerful, can add and remove properties on the fly
- delete operator to delete own property, see delete operator

#### Optional chaining

- optional property accessing and function calls (with ES2020)
- returns `undefined` if reference to left is nullish, instead of throwing `TypeError` that can't read property of `undefined`
- doesn't need to guard chain anymore using `&&`, non-nullish falsy values `false`, `0`, `NaN`, `""` are allowed unlike with `&&` 🎉
- works with dot and bracket notation, i.e. with arrays use bracket notation

```javascript
const p = {};

const q = p.first.second; // Uncaught TypeError: Cannot read property 'second' of undefined
const q = p.first && p.first.second; // undefined
const q = p.first?.second; // undefined
```

```javascript
const p = {};

const q = p["first"]["second"]; // Uncaught TypeError: Cannot read property 'second' of undefined
const q = p["first"] && p["first"]["second"]; // undefined
const q = p["first"]?.["second"]; // undefined
```

- short-circuit evaluation, doesn't evaluate further to right if returns early
- can stack multiple optional chaining operators in a property accessor sequence, also mixing dot and bracket notation
- beware: don't use if value is guaranteed to not be nullish, would hide bugs since doesn't throw anymore
- only checks if value to left is nullish, not whole chain

```javascript
const p = {};

const q = p.first.second?.third; // Uncaught TypeError: Cannot read property 'second' of undefined
const q = p?.first.second; // Uncaught TypeError: Cannot read property 'second' of undefined
```

- works with method calls as well

```javascript
const p = {};

const q = p.first(); // Uncaught TypeError: o.first is not a function
const q = p.first?.(); // undefined
```

### Getters and Setters

```javascript
const car1 = {...
              owners: ["Bob", "Sarah", "James"], // owners, first to current
              set currentOwner(name) {owners += name;},
              get currentOwner() {return owners[owners.length - 1];}
             }
console.log(car1.currentOwner); // "James"
car1.currentOwner = "Zoe";      // Zoe just bought the car
console.log(car1.currentOwner); // "Zoe"
```

- bind object's property to a function call
- behaves like any other property, can be accesses, set, or deleted, but is itself not a property, e.g. there is no property `currentOwner` in above example, it's just getters and setters
- can make property seem like it stores dynamically computed value
  beware: using getters and setters one can't trust anymore that an object's properties stay the same if they haven't been modified, since now they can be modified internally through different "properties", use cautiously ❗️
- set: takes exactly one argument, no return statement
- get: takes exactly zero arguments, has return statement
- can not have a normal property with the same name as get / set
- use `Object.defineProperty()` to define getters and setters for existing object

### Property attributes

- define if a property is writable, enumerable and deletable
- by default a property is writable, enumerable and deletable, but not necessarily the built-in ones ❗️
- attributes differ for data property (static, normal value) or accessor property (dynamic, getters and setters)

#### Descriptor object

- used to specify attributes
- both data and accessor descriptor objects can contain the following properties
  - `enumerable`: boolean, property shows up during enumeration, e.g. in `for..in` or `Object.keys()`
  - `configurable`: boolean, property can be deleted and data property can be changed to accessor or vice versa
- data property descriptors can also contain the following properties
  - `value`: any type, value of property
  - `writeable`: boolean, property value can be modified
- accessor property descriptors can also contain the following properties
  - `get`: function, getter for property
  - `set`: function, setter for property

#### Setting property attributes

- `Object.defineProperty()`: defines new property on an object or modifies existing one, can specify property descriptor, returns object
- for new property, `enumerable`, `configurable` and `writeable` default to `false` if not specified with a property descriptor, i.e. property is read-only, non-enumerable and non-deletable
- for new property, `value`, `get` and `set` default to `undefined` if not specified with a property descriptor, i.e. property value is `undefined`
- for existing property, attributes not specified with a property descriptor default to their existing value
- a descriptor containing only `enumerable` and `configurable` is treated as data descriptor

```javascript
const p = {};

// data descriptor
Object.defineProperty(p, "name", {
  value: "Peter",
  writeable: true,
  enumerable: true,
  configurable: true
})

Object.keys(p); // ["name"]

// data descriptor
Object.defineProperty(p, "name", {
  enumerable: false
})

Object.keys(p); // []
```

```javascript
let n = 42;

const p = {};

// accessor descriptor
Object.defineProperty(p, "age", {
  get() { return n; }, // shorthand with ES6
  set(age) { n = age; }, // shorthand with ES6
  enumerable: true,
  configurable: true
})
```

- `writable` once set to `false` can not be reset to `true`, integrity is guaranteed  
beware: resetting it fails silently even if property is non-configurable ⚠️ ⚠️ ⚠️
- `enumerable` once set to `false` can be reset to `true`, but resetting it fails on non-configurable property fails with `TypeError: Cannot redefine property`
- `configurable` once set to `false` can not be reset to `true`, integrity is guaranteed, resetting it fails with `TypeError: Cannot redefine property`
- in sloppy mode writing to a non-writable property or deleting a non-configurable property doesn't throw an error, use strict mode instead ⚠️



## Constructors

- regular function, but designed to be called with `new` keyword
- convention is to name with upper CamelCase

### Terminology

- object type: abtract notion of type of object, e.g. cars, trucks, bikes
- constructor: blueprint for object type, like a custom object type, e.g. `Car` constructor creates car objects
- instance: object created using a particular constructor, e.g. `car1` is instance of `Car`

### `this` keyword

- `this` is a dynamically bound reference to an object, like a variable
- object is determined by the context of the function call, not by how the function was defined ⚠️
- allows to reuse one function in different contexts, makes it flexible, share one function on infinitely many instances, but often source of confusion
- outside of any function in global context `this` is the global object
- in a function `this` is dependend on the call site, by the following precedence:
  1. in a constructor, i.e. called by `new` operator, `this` refers to the object being constructed
  2. in a function called using `Function.prototype.call`/`apply()`, or created using `Function.prototype.bind()`, `this` refers to the object specified as first argument to those methods
  3. in a method, `this` is the object the method is called on, doesn't matter where function was defined, outside, on prototype chain, etc. ❗️
  4. in a function called without any of the above, `this` is the global object in non-strict mode or `undefined` in strict mode
- beware: in an event handler, `this` is the object the event handler is attached to, is not necessarily the same object the event targeted, see Events
- beware: `this` depends only on how function was called, not how it was defined, e.g. if a method is passed as value and then called independent from object, it's `this` will be back to default ⚠️

```javascript
// invoked as function
function sayHi() {
  console.log("Hi, my make is " + this.make);
  };
sayHi(); // "Hi, my make is undefined"

// invoked as method
car1.sayHi = sayHi;
car1.sayHi(); // "Hi, my make is Honda"

// invoked as function even though defined as method
const func = car1.sayHi
func(); // "Hi, my make is undefined"

// invoked as function even though defined as method
function execute(callback) {
  callback();
}
execute(car1.sayHi); // "Hi, my make is undefined"

// "undefined" since "this" is global object in non-strict mode which has no property "make"
```

- use `Function.prototype.call`/`apply()` to call any function with `this` being specified, or create entirely new function using `Function.prototype.bind()` with `this` being fixed

```javascript
func.call(car1); // "Hi, my make is Honda"
func.apply(car1); // "Hi, my make is Honda"
func(); // "Hi, my make is undefined"

const func2 = car1.sayHi.bind(car1);
func2();  // "Hi, my make is Honda"
```

### `new` operator

- operates on a function, but used specifically with constructor or class, e.g. `new Constructor();`
- usually assigns it to a variable
- steps of the `new` operator:
  1. creates a new empty object
  2. sets its `[[prototype]]` property to the constructor's `prototype` property (see later)
  3. calls the constructor with `this` being the new object
  4. returns `this` (if the function doesn't return it's own object)

### Constructor function

```javascript
function Car(make, model, year) {
  this.make = make;                               // property
  this.model = model;                             // property
  this.year = year;                               // property
  this.honk = function() {console.log("Hooonk");};// method
  this.beep = function() {console.log("Beeep");}; // method, no shorthand
  this.blub = alreadyExistingFunction;            // method
}

/*const car1 =*/ new Car("Honda", "Civic", 1998)
```

- constructor is simply a function that sets object properties of object in `this`, as if setting any other object's properties, just object has weird name "this"
- when called via `new` operator, it exactly sets all the properties for the newly created object
- constructor has no return statement, otherwise calling it with `new` doesn't make sense, the newly created object would not be used (see step 4. in steps of the `new` operator), wouldn't be a constructor
- use dot notation, because the property names are _not_ computed, i.e. only the assigned `make` becomes the argument variables value, not the `make` in `this.make`
- object created with constructor is identical to object literal equivalent, except when using constructor also inheritance is set up (see later)  
  -> use constructor if wants to create many objects that inherit, object literal if only needs single generic
- syntax is different from object literal, semicolons instead of commas, equal signs instead of colons
- beware: forgetting `new` operator when calling constructor makes it modify the properties of the global object in non-strict mode without any error ⚠️ always use in strict mode, or better use classes or `Object.create()`

### Static properties

- properties of constructor itself, since it's like any other object can add properties to it
- not inherited to any instances, since constructor itself is not part of the instances' prototype chain, only the constructor's `prototype` property
- usually uses for methods instead of properties since they stay the same for all instances
- in a static method, `this` refers to the constructor, since the static method is called as method of the constructor, i.e. `this` is not an object instance ❗️

```javascript
Car.repairPrice = 420;
Car.repair = function(car) {
  console.log("Repair done. " + car.make + " " + car.model + " looks like new.");
  console.log("Cost " + this.repairPrice); // this will be Car itself
};

Car.repair(car1); // "Repair done. Honda Civic looks like new.", "Cost 420"

console.log(car1.repairPrice); // undefined, no such property in prototype chain of car1
```



## Inheritance


### Idea

- deriving new objects from existing objects
- need to specify only what's different, inherit what's the same, i.e. not one huge constructor for all types of objects and neighter one full constructor for every single type of object, e.g. `SportsCar` from `Car` from `Vehicle`
- allows for code reuse, better scalability, maintainability, readability, etc.

### Prototypal Inheritance

- JS uses a prototype-based inheritance model, as opposed to the traditional class-based inheritance model, single mainstream programming language to date that does this
- in class-based languages objects are instances of class blueprints, classes can inherit from other classes, objects can not be changed
- JS is class-free, objects are the only player, objects inherit directly from other objects, objects themselves are "prototypes" from which other objects can inherit
- inheritance of objects is done via links between them, are not copies of class blueprints, if one changes the other changes as well, instead of copying behavior down the inheritance chain delegates it up the chain
- should call it "delegation" instead of "inheritance" since doesn't copy but borrow properties, or differential inheritance because object contains only what's different
- prototype-based system is superset of class-based system, i.e. can implement classes in prototype-based system but not the other way around
- offers more flexibility than traditional class-based inheritance, can freely add / remove properties to individual objects without having to modify whole class, allows to modify prototype objects at run-time to change inherited properties, allows to change whole inheritance chain at run-time
- does not support multiple inheritance, i.e. every object inherits only from exactly one other object

#### Pseudoclassical pattern

- JS doesn't embrace its prototypal nature that objects inherit directly from other objects, instead it tries to look class oriented
- implements constructor functions to look like classes and the `new` operator so can "create" objects from them
- but constructor doesn't create instance, are both just equally objects that are linked
- with actuall `class` syntax makes even more confusing

### The `[[prototype]]` property

- each object has a hidden internal property called `[[prototype]]`, which is set to another object, the object's "prototype" object, hidden link
- since objects are addressed by reference, any changes made to this prototype object will show up in the `[[prototype]]` property
- since the prototypes are objects themselves, they have their own `[[prototype]]` properties, linking to another prototype object, this chain is called a "prototype chain", the end of the chain is always the `null` object, `null` is the last prototype / commin ancestor to all objects
- any properties added to a prototype object will be available to all objects below in the prototype chain
- when accessing an object's property, JS first searches the own properties of the object, if no matching property is found it goes up the prototype chain searching through every prototype object until the first match, if it reaches `null` it returns `undefined`, i.e. doesn't need to specify where in the prototype chain the property is, can use property as if it were own property of object  
  -> Prototypal inheritance is actually more "delegation" than inheritance, since properties are only referenced and not copied  
  -> for performance reason don't let JS search long prototype chains
- since JS returns the first match when searching the prototype chain for a property, later properties "overshadow" earlier ones with the same name

```javascript
car1.color;                   // undefined, no such own property or in prototype chain
Car.prototype.color = "black";// set default color for all cars
car1.color;                   // "black", from prototype chain
car1.color = "red";           // add new own property "color"
car1.color;                   // "red", overshadows any "color" property in prototype chain
delete car1.color;            // "true", deletes own "color" property
car1.color;                   // "black", from prototype chain
delete car1.color;            // "true", even though nothing to delete since no own "color" property exists anymore
car1.color;                   // "black", from prototype chain
```

- there can be various prototype chains, all ending at `null`, see later

![prototype chain - first simple picture](opcfirst.svg)

- problem 1: by default the `[[prototype]]` properties of the objects are not chained to each other, would need to set it for each single object
- problem 2: changing the `[[prototype]]` property of an object is very inefficient operation, should not be done manually
- solution: constructors or `Object.create()`, see later

### The `prototype` property

- functions are objects and have like every other object a `[[prototype]]` property linking to a prototype object
- but they are special, because they have a second property called `prototype` which is set to yet another prototype object, can be the same prototype object as the `[[prototype]]` property, e.g. like the `Function` constructor has is, see later
- this other prototype object is part of a prototype chain as well
- this prototype object also has a property called `constructor` set to the function object for which it is a prototype, i.e. creating a circular reference
- when creating an object using a constructor, the `new` operator sets the `[[prototype]]` property of the new object to the `prototype` property of the constructor, i.e. exactly this second prototype object, (see step 2. in steps of the `new` operator) 
- so all instances of a constructor get this same object as their `[[prototype]]`, any changes made to it are instantly available to all instances, need only to add / change / remove properties in this single object accessible through constructor's `prototype` property  
  -> enables inheritance
- can access constructor from every instance through the `constructor` property of the constructor's `prototype` property which is inherited to every instance, e.g. useful if doesn't know constructor's name but has a instance available ❗️

```javascript
const car2 = new Car("Toyota", "Corolla", 1995);  // via constructor
const car2 = new car1.constructor(/*...*/);       // via instance, same result
```

![protoype chain - second closer picture](opcsecond.svg)

- by default a function's `prototype` property is linked to the `prototype` property of `Object` which is linked to `null`, see later how can change `prototype` property of constructors for multi-level inheritance
- object literals have their `[[prototype]]` property directly set to `Object.prototype`

![protoype chain - third complete picture](opcthird.svg)

### Summary

- every object has exactly one `[[prototype]]` property (orange arrow)
- every function object has an additional `prototype` property (pink arrow)
- `Object.prototype` is a common prototype of every typical object, (see later how can create different prototype chains using `Object.create()`)
- beware: don't simply add custom properties to `Object.prototype` since it pollutes the entire global prototype chain, instead create a single constructor for all your objects to inherit from and use its `prototype` propery to add desired properties
- a function object has by default `Function.prototype` as `[[prototype]]` (blue objects)
- an object literal has by default `Object.prototype` as `[[prototype]]`
- "own properties" are properties of object itself, not from prototype chain
- all functions have the additional `prototype` property even though they might not be used as constructors at all, any instances of a constructor function get their `[[prototype]]` property linked to that `prototype` property
- the prototype object confusingly has no own name and is only accessible through the constructor's `prototype` property, e.g. `Car.prototype`
- calling the constructor function "constructor" is a bad name since it doesn't "construct" the object, it just provides access to its prototype and can already fill the instance with some properties, the `new` operator does the actual construction
- → pseudoclassical pattern obscures the prototypal inheritance by introducing a detour via constructor functions

### Accessing `[[prototype]]` property

- the `[[prototype]]` property of an object is internal, can not be accessed like a normal property
- `Object.get`/`set.PrototypeOf()`: prefered way
- `Object.prototype.__proto__`: legacy, is actually a getter / setter property that references instance via `this` binding
- modifying the `[[prototype]]` of an object is a very slow operation, avoid if possible, best way of changing it is to leave it and create new object with desired prototype using `Object.create()` (see later)
- can access `prototype` properties of constructor without knowing its name by walking up the inheritance chain, can modify or delete properties

```javascript
Object.getPrototypeOf(car1).sayHi = function() {/*...*/} // equivalent to Car.prototype.sayHi
car1.sayHi(); // "Hi, my make is Honda"
delete Object.getPrototypeOf(car1).sayHi
car1.sayHi(); // TypeError: car1.sayHi is not a function, doesn't exist
```

### Inheriting properties

- a constructor's `prototype` properties are available on every object instance of descendant object type, e.g. everything in `Object.prototype` is available in every object (as long as it's part of the standard prototype chain, see `Object.create()` later how to create different prototype chains)
- add properties to custom constructor's prototype property, because
  - are available to all instances like own properties, but beware property shadowing
  - only way if wants to easily modify / add / remove later for all instances
  - exist only once in prototype object instead of being copied to every instance

```javascript
Car.prototype.sayHi = function() {console.log("Hi, my make is " + this.make);};
car1.sayHi(); // "Hi, my make is Honda"
car2.sayHi(); // "Hi, my make is Toyota"
```

- usually adds only properties to object instances and all methods to constructor's `prototype` object, since methods usually don't change for each object instance, and even later members in inheritance chain can access them
- don't pollute default prototype chains by adding to built-in constructor's `prototype` properties, e.g. `Object.prototype`, instead create a single constructor for all your objects to inherit from and use its `prototype` propery to add desired properties
- properties added to constructor object itself instead of it's `prototype` property won't inherit to instances
- remember: `this` in a method refers to the calling object, i.e. especially not the prototype object in which a method is defined and inherited from, e.g. like in `car1.sayHi()` above

### Data types as objects

- all data types in JS either are or behave like objects
- for each data type exists a constructor, holds relevant properties in its `prototype` property for all instances to inherit, e.g. `Object`, `Function`, `Array`, `String`, `Number`, `Boolean`
- data type constructors `Array`, `String`, `Number`, `Boolean` are like `Car` in above diagram, have their `prototype` properties linked to `Object.prototype` and their `[[prototype]]` properties to `Function.prototype`
- objects, functions, and arrays are objects, even if created using literals, no difference if created literally or using data type constructor
- strings, numbers, and booleans are primitives if created literally, not objects, different from data type constructors which create objects
- beware: don't create primitives using constructors, don't work as expected, use literals instead, also don't create functions using contructor see [Functions](#) ⚠️

```javascript
"Hi" === new String("Hi") // false, since RHS is of type object
Boolean(false) === true   // true, since object is always truthy
```

- when using strings, numbers, or booleans like an object, e.g. accessing a method, JS silently creates a temporary "wrapper object" to perform the desired operation on, i.e. makes primitives behave like objects

```javascript
"Hello World!".toUpperCase() // "HELLO WORLD!"
"  boat  \n".trim()          // "boat"
3.141592654.toFixed(2)       // "3.14"
3.141592654.toPrecision(3)   // "3.14"
```

- wrapper objects are only one time use, are discarded immediately after use, i.e. can't use like objects to store data

```javascript
const str = "Hello";
str.data = "abc";
console.log(str.data); // undefined, already discarded
```

- data type constructors implement a `valueOf()` method in their `prototype` property, to overshadow `Object.valueOf()`, returns the primitive value of the wrapper instance, e.g. `Boolean(false).valueOf()` gives primitive false value, `(new Number (42)).valueOf()` gives primitive `42`
- data type constructors implement a `toString()` method in their `prototype` property, to overshadow `Object.prototype.toString()`, returns value as string, useful for all data types except for objects, returns useless "[object type]", can use `JSON.stringify` instead to stringify object



## Multi-level inheritance

### Idea

- link `prototype` properties of multiple constructors in row, see diagram below
- in child constructor could make use of parent constructor to set properties
- set child constructor's `prototype` property to parent constructor's `prototype` property to build inheritance chain
- instead of detour via constructor functions can link up individual objects directly using `Object.create()`, closer to real prototypal inheritance, but focus here on constructors

### Properties

```javascript
function SportsCar(..., zeroTo60) {
  Car.call(this, ...);
  this.zeroTo60 = zeroTo60;
}

const sportsCar1 = new SportsCar(..., 3.8);

console.log(car1.zeroTo60); // undefined
console.log(sportsCar1.zeroTo60); // 3.8
```

- in child constructor call parent constructor to set up existing properties, e.g. `make`, `model` and `year` from `Car`
- need to use `Function.prototype.call`/`apply()` to specify correct `this` in parent constructor, otherwise `this` would be back to default, would modify the global object in non-strict mode ⚠️
- set any additional own properties for this object type, e.g. a `zeroTo60` property just for sports cars
- until now prototype chain is not set up yet, has just set properties of object instance, e.g. `SportsCar.prototype` links still to `Object.prototype`
- beware: using global variables in parent constructor, parent constructor will also be called any time a child object instance is created, e.g. incrementing an id counter ❗️
- child objects don't necessarily need to have same properties as parent objects, could skip above and just set own properties, i.e. if object types don't have same properties but should still inherit methods from each other

### Prototype chain

```javascript
SportsCar.prototype = Object.create(Car.prototype);
SportsCar.prototype.constructor = SportsCar;
```

- set child constructor's `prototype` property to link to its parent constructor's `prototype` property
- use `Object.create()` to create new empty object with parent constructor's `prototype` property as `[[prototype]]`, use it as `prototype` property of the child constructor
- don't just set to parent constructor's `prototype` properties, would share same prototype, no inheritance chain, makes no sense, e.g. `SportsCar.prototype = Car.prototype` ⚠️
- don't just set it a new instance of parent constructor, would have all instance properties, inherited to all child objects, e.g. `SportsCar.prototype = new Car()` ⚠️
- don't just manually change the `[[prototype]]` property of the child constructor's `prototype` property to point to the parent constructor's `prototype` property, very inefficient, e.g. `Object.setPrototypeOf(SportsCar.prototype) = Car.prototype` ⚠️
- need to correct the `constructor` property within the child constructor's new `prototype` property to point back to it, (would't be necessary if directly changed prototype, but won't ever do that)
- JS does not support multiple inheritance because any object can have only exactly one `[[prototype]]` property

```javascript
function SportsCar(..., zeroTo60) {
  Car.call(this, ...);
  this.zeroTo60 = zeroTo60;
}

SportsCar.prototype = Object.create(Car.prototype);
SportsCar.prototype.constructor = SportsCar;

SportsCar.prototype.scream = function() {console.log("Brummm");};

const sportsCar1 = new SportsCar(..., 3.8);
sportsCar1.honk(); // "Hooonk", from Car.prototype
sportsCar1.scream(); // "Brummm", from SportsCar.prototype
```

![protoype chain - fourth multi-level picture](opcfourth.svg)

- accessing properties may go up the prototype chain, but `this` points back at instance, is rooted at call site, can reuse methods no matter where they are on the prototype chain, i.e. this-aware methods on prototype chain work as if they were on instance

```javascript
function Car(...) {
  /* ... */
}

Car.prototype.sell = function() {
    console.log(this.sellingPrice != null ? `You can buy me for ${this.sellingPrice}€.` : "This car is not sold.");
}

function UsedCar(..., sellingPrice) {
  Car.call(this, ...);
  this.sellingPrice = sellingPrice;
}

UsedCar.prototype = Object.create(Car.prototype);
UsedCar.prototype.constructor = UsedCar;

const car1 = new Car(...);
car1.sell(); // "This car is not sold."

const car2 = new UsedCar(..., 420);
car2.sell(); // "You can buy me for 420€."

// Even though the method sell() sits in the prototype chain on Car.prototype, 'this' always points back down to the instance it was called on, which here is an instance of UsedCar.
```



## Classes

- mostly syntactical sugar to make prototype-based inheritance look more class-oriented (with ES6)
- beware: they still work differently than in class-oriented languages ⚠️
- automate some tasks like setting up multi-level inheritance chain, bundle code together, cleaner than verbose constructor function syntax 🎉
- are just special constructor functions
- not hoisted ❗️
- class body is in strict mode by default ❗️
- error if called without `new`, i.e. no accidental modification of global oject 🎉
- rarely uses constructor functions anymore in favor of classes, but needed to understand them to understand how classes work

### Class constructor

- can use like normal constructor
- `constructor` method is called with arguments of class
- if no `constructor` method is specified, it defaults to an empty function
- don't confuse `constructor` method with `constructor` property in classe's `prototype` property ❗️

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;                               // property
    this.model = model;                             // property
    this.year = year;                               // property
    this.honk = function() {console.log("Hooonk");};// method
    this.beep = function() {console.log("Beeep");}; // method, no shorthand
    this.blub = alreadyExistingFunction;            // method
  }
}

/*const car1 =*/ new Car("Honda", "Civic", 1998)
```

### Instance methods

- methods in the class other than `constructor` are directly put into the instance's `prototype` property
- no need to add them separately to constructor's `prototype` property afterwards dangling behind in code 🎉
- use only ES6 method shorthand notation
- no comma between class methods
- any assigned variables will be interpreted as instance properties as if they were in constructor, see Instance fields, i.e. assigned functions including function expressions and arrow functions are put on instance itself ❗

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
  }
  
  honk() {console.log("Hooonk");}
  beep() {console.log("Beeep");}
  blub = alreadyExistingFunction // instance method
}

const car1 = new Car("Honda", "Civic", 1998);
car1.honk(); // "Hooonk", from Car.prototype
```

- can make private - not visible outside class - by prepending name with `#` symbol (with ES2022)
- private methods are separate from non-private ones, can have private and non-private with same name, otherwise would break encapsulation
- private methods are scoped to containing class, i.e. subclass can have different method with name of private method in parent class

```javascript
class Person {
    sayHi() {
        console.log("Hello world!");
    }
    #sayHi() {
        console.log("Wello horld!");
    }
}
```

### Instance fields

- properties declared outside of constructor (with ES2022)
- behave as if they were inside constructor, i.e. get created for every instance, are not on prototype
- to look more like classical syntax
- also called "class fields"

```javascript
// sugar syntax
class Car {
  constructor(make, model, year) {
    /* ... */
  }

  type = "Car";
}

// real effect
class Car {
  constructor(make, model, year) {
    /* ... */
    this.type = "Car";
  }
}
```

- can make private - not visible outside class - by prepending name with `#` symbol, must declare it outside of constructor
- private fields are separate from non-private ones, can have private and non-private with same name, otherwise would break encapsulation
- private fields are scoped to containing class, i.e. subclass can have different field with name of private field in parent class
- private fields are only accessible using dot notation, not bracket notation, i.e. also not computed property keys ❗️

```javascript
class Person {
    #name = "Batman";
    #age;
    constructor(name, age) {
        this.name = name;
        this.#age = age;
    }
}
```

### Static methods

- methods of class object itself, instead of instance
- no need to add them separately to constructor object afterwards dangling behind in code 🎉
- can make private - not visible outside class - by prepending name with `#` symbol (with ES2022)

```javascript
class Car {
  constructor(make, model, year) {
    /* ... */
  }
  
  /* ... */

  static repair(car) {
    console.log("Repair done. " + car.make + " " + car.model + " looks like new.");
  }
}

const car1 = new Car("Honda", "Civic", 1998);
Car.repair(car1); // "Repair done. Honda Civic looks like new."
```

### Static fields

- properties of class object itself, instead of instance (with ES2022)
- no need to add them separately to constructor object afterwards dangling behind in code 🎉
- can make private - not visible outside class - by prepending name with `#` symbol

```javascript
class Car {
  constructor(make, model, year) {
    /* ... */
  }
  
  /* ... */

  static repairPrice = 420;
  static repair(car) {
    console.log("Repair done. " + car.make + " " + car.model + " looks like new.");
    console.log("Cost " + this.repairPrice + "€"); // 'this' will be Car itself since repair is invoked as Car.repair()
  }
}

const car1 = new Car("Honda", "Civic", 1998);
Car.repair(car1); // "Repair done. Honda Civic looks like new.", "Cost 420€"
```

### Multi-level inheritance

- use `extends` to let child class instances inherit properties of parent class instances
- automatically sets up prototype chain, no need to set the constructor's `prototype` property manually anymore 🎉
- parent class constructor is called via `super` keyword, automatically passes the correct `this`, no need to specify it with `Function.prototype.apply`/`call()` anymore 🎉

```javascript
class SportsCar extends Car {
  constructor(..., zeroTo60) {
    super(...);
    this.zeroTo60 = zeroTo60;
  }
  
  scream() {console.log("Brummm");}
}

const sportsCar1 = new SportsCar(..., 3.8);
sportsCar1.honk();   // "Hooonk", from Car.prototype
sportsCar1.scream(); // "Brummm", from SportsCar.prototype
```

- if no constructor is specified in the child class it uses an empty constructor with a super call

```javascript
constructor(...args) {
  super(...args);
}
  ```

- to use `this` in the constructor of a derived class, `super()` must have been called before ❗️
- since `super()` is called first, can overwrite any properties set by parent class, and overshadow any methods of parent classes prototype  
  see Constructors to remember how setting properties and methods work ❗️

```javascript
class Named {
    constructor() {
        this.name = "foobar";
    }
    sayHi() {
        console.log("Hello World!");
    }
}

class Person extends Named {
    constructor(name) {
        super();
        this.name = name;
    }
    sayHi() {
        console.log(`Hi, my name is ${this.name}.`);
    }
}

const q = new Named();
q.sayHi(); // Hello World!

const p = new Person("Peter");
p.sayHi(); // Hi, my name is Peter.
```

### Mixins

- a class containing methods to be used by other classes without having to be parent class of those other classes
- used for sharing the same tools across different classes
- since JS doesn't support multiple inheritance, needs to use mixin as parent class, becomes part of inheritance chain ❗️

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log(`Hi, my name is ${this.name}. I have ${this.hat ? 'a' : 'no'} hat and ${this.shoe ? 'a' : 'no'} shoe.`);
  }
}

function Hat(classToExtend) {
  return class extends classToExtend {
    constructor(...args) {
      super(...args);
      this.hat = true;
    }
  }
}

function Shoe(classToExtend) {
  return class extends classToExtend {
    constructor(...args) {
      super(...args);
      this.shoe = true;
    }
  }
}

const PersonWithHat = Hat(Person);
const PersonWithShoe = Shoe(Person);
const PersonWithHatAndShoe = Shoe(Hat(Person));

const p = new Person("Peter");
p.sayHi(); // Hi, my name is Peter. I have no hat and no shoe.

const q = new PersonWithHat("Jim");
q.sayHi(); // Hi, my name is Jim. I have a hat and no shoe.

const r = new PersonWithShoe("Lisa");
r.sayHi(); // Hi, my name is Lisa. I have no hat and a shoe.

const s = new PersonWithHatAndShoe("Sarah");
s.sayHi(); // Hi, my name is Sarah. I have a hat and a shoe.
```



## Built-in constructors

- there are many built-in object types, the data type wrappers, `Math`, `Date`, `RegExp`, `JSON`, `Intl`, error object types, etc.
- they can construct instances, have built-in own properties, or inherit properties to all their descendants via their `prototype` property
- Browser APIs add many more object types

### `Object` object type

#### Constructor

- `Object()`: object constructor
- equivalent to object literal

#### Properties

<!-- todo: add missing, like Object.assign() -->
-  `Object.create()`: creates new object with argument as `[[prototype]]`, only prototype properties on custom prototype chain are available, i.e. if argument is `null` creates empty prototype chain, collision free from otherwise built-in properties like "constructor" ❗️
-  `Object.getOwnPropertyNames()`: returns array with own property names including non-enumerable ones, but not Symbols
-  `Object.keys()`: returns array with own enumerable string-keyed property names as strings, can use any array methods on it like iteration 🎉, not in `Object.prototype` because of fear of collision with existing code
- `Object.values()`: returns array with own enumerable string-keyed property values, complement to `Object.keys()` (with ES2017)
- `Object.entries()`: returns array with own enumerable string-keyed property `[key, value]` pairs, complement to `Object.keys()` and `Object.values()` (with ES2017)
-  `Object.get`/`setPrototypeOf()`: get / set `[[prototype]]` property of an object, slow, don't use ❗️instead create new object with desired `[[prototype]]` using `Object.create()`
- `Object.defineProperty`/`ies()`: add property/ies to object, can specify property descriptor, returns object
- `Object.getOwnPropertyDescriptor()`: returns property descriptor for own property
- `Object.getOwnPropertyDescriptors()`: returns property descriptors for all own properties (with ES2017)
- `Object.getOwnPropertyNames()`: returns names of own properties including non-enumerable ones, except of symbol properties
- `Object.getOwnPropertySymbols()`: returns symbols of own properties including non-enumerable ones
- `Object.preventExtensions()`: disallows adding new properties, once non-extensible an object can not be made extensible again  
  beware: does not prevent deletion of properties, also can be "added" via prototype chain ❗️
- `Object.seal()`: makes object non-extensible and all properties non-configurable, i.e. can not add or remove properties anymore, once sealed an object can not be unsealed again  
- `Object.freeze()`: seals object and makes all properties non-writable, i.e. properties are immutable, once freezed an object can not be unfreezed again  
  beware: only shallow freeze, nested objects can still be mutated ❗️
- `Object.isExtensible()`: checks if object is extensible
- `Object.isSealed()`: checks if object is sealed
- `Object.isFrozen()`: checks if object is frozen

#### Prototype properties

- `Object.prototype.constructor`: reference to constructor function of object, available on every object, overshadowed in constructor's `prototype` property
- `Object.prototype.__proto__`: reference to `[[prototype]]`, deprecated, use `Object.get`/`setPrototypeOf()` instead, 🚫
- `Object.prototype.hasOwnProperty()`: true if object has given own property, else false
- `Object.prototype.isPrototypeOf()`: checks if given object is part of the object's prototype chain, (instanceof operator checks if `Constructor.prototype` is part of the object's prototype chain)
- `Object.prototype.toString()`: string representing an object, not useful since only "[object type]", can use `JSON.stringify` instead to stringify object, or overshadow it on constructor's prototype
- `Object.prototype.valueOf()`: primitive value of an object, useful for primitive data type wrappers, overshadowed anyways

### `Date` object type

- dates are stored as milliseconds since 01.01.1970, 00:00:00 UTC ("Unix epoch")
- create date object using `new Date(...)`
- many inherited properties and methods available to convert dates
- `Date()`, i.e. like forgetting `new`, returns current date and time as string, different behavior than normal constructor who would modify global object ❗️
- `Date.now()` returns number of milliseconds since epoch as string

### `RegExp` object type

- regular expression constructor takes string, can use dynamically,
- second parameter is string containing flags `g`, `i` and `m`
- needs to string escape backslash and quotes ⚠️
- use regular expression literal whenever possible, enclosed in forward slashes, flags can be appended
- RegEx has no support for internationalisation, needs to pick Unicode characters yourself
- use multiple capturing groups and select only desired ones, instead of using complex positive / negative lookahead / lookbehind

<!-- ToDo: properties can be set to enumerable, writable, configurable etc. "descriptor" -->



## Resources

- MDN - as usual
- [V8 - Optional chaining](https://v8.dev/features/optional-chaining)