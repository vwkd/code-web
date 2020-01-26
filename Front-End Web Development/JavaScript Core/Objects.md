# Objects

[TOC]

<!-- ToDo: write content -->



#### constructor

- constructor's properties are available on every object instance of descendant object type



#### `this` keyword

- if a function is passed as argument to another function and called only within it, `this` in the function again refers to the default value (global object / undefined in non / strict mode), i.e. `this` in a method won't refer to its object anymore if it is passed as argument to another function ⚠️

  ```javascript
  function sayHi() {console.log("Hi " + this.name)};
  const person = {name: "Tom", sayHi};
  person.sayHi(); // "Hi Tom", this ≙ person
  
  function otherFunc(callback) {
    callback();
  };
  otherFunc(person.sayHi); // "Hi ", this ≙ window
  ```