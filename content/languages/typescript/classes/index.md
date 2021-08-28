---
title: Classes
author: vwkd
index: 5
tags:
  - typescript
---
# Classes



## Introduction

- class fields (≙ instance properties) types are declared outside of constructor, matches syntax of classical languages, not needed for inherited properties

```typescript
// TypeScript
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}
```

```javascript
// compiled JavaScript
class Person {
    constructor(name) {
        this.name = name;
    }
}
```

- can leave out constructor if initialises with value

```typescript
// TypeScript
class Person {
    name = "Peter";
}
```

```javascript
// compiled JavaScript
class Person {
    constructor() {
        this.name = "Peter";
    }
}
```

- can limit accessibility of properties and methods by prefixing with modifiers, but only applies within TS, gets stripped away with compilation ❗️
    - `public`: property is available inside and of outside class, like default JS
    - `private`: property is available only inside class, if makes constructor itself private the class can not be instantiated outside or extended  
      beware: even though behaves like `#` within TS, doesn't compile to `#` in JS ❗️
    - `protected`: like `private`, but allows use in derived classes, if protects constructor itself the class can not be instantiated outside but extended
    - `readonly`: property can be written to only from within constructor, not other method or outside, but can still be read from outside

```typescript
// TypeScript
class Person {
    public name: string;
    constructor(name: string) {
        this.name = name;
    }
}
```

```javascript
// compiled JavaScript
class Person {
    constructor(name) {
        this.name = name;
    }
}
```

- parameter properties: instance property shorthand by prefixing constructor parameter directly with any of the modifiers, makes type of class field outside of constructor obsolete

```typescript
// TypeScript
class Person {
    constructor(public name: string) {
    }
}
```

```javascript
// compiled JavaScript
class Person {
    constructor(name) {
        this.name = name;
    }
}
```

- `static` properties can be used with same modifiers, but not use a type parameter from a generic class

```typescript
// TypeScript
class Person {
    static fullname = "Peter";
}
```

```javascript
// compiled JavaScript
class Person {}

Person.fullname = "Peter";
```

- constructors themselves can not be generics or have return types
- can specify overloads for constructor
- use `strictPropertyInitialization` flag to demand initialisation of class fields to not forget
- use definite assignment assertion operator `!` after field name to assert it will be initialised, e.g. through calling some method
- a class automatically creates a type representing the classes' instances, i.e. can use like interfaces, type aliases, etc.
- two classes containing identical private or protected properties are _not_ compatible types

```typescript
class Person {
    constructor(private name: string) {
    }
}

class Employee {
    constructor (private name: string) {
    }
}

const p: Employee = new Person("Peter"); // Types have separate declarations of a private property 'name'.ts(2322)
```

- a getter without a setter is automatically inferred to be `readonly`
- the getter type must be a subtype of the setter type



## Inheriting classes

- use `implements` to check that a class instance implements a type
- can implement multiple types, separated by comma or after each other
- doesn't change class at all, just error checking, don't confuse with `extends` ❗️

```typescript
interface Named {
    name: string;
}

class Person implements Named {
    name = "Peter";
}

// Class 'Car' incorrectly implements interface 'Named'.
// Property 'name' is missing in type 'Car' but required in type 'Named'.(2420)
class Car implements Named {
    speed = 42;
}
```

- beware: only checks against class instance, i.e. can't check if implements given static properties or constructor ❗
- can implement constructor check using class expressions

```typescript
interface Named {
    name: string;
}

interface PersonConstructor {
    new (name: string): void;
}

const Person: PersonConstructor = class Person implements Named {
    name: string;

    constructor(name: string) {
        this.name = name;
    }
}
```

- can use together with `extends`, first `extends` then `implements`
- use `override` on a method in child class to guarantee that it exists in parent class, doesn't add new one if doesn't exist, e.g. after rename, after deleted, etc.

```typescript
class Parent {
    // foo() {}
}

class Child extends Parent {
    // This member cannot have an 'override' modifier
    // because it is not declared in the base class 'A'.(4113)
    override foo() {}
}
```



## Abstract classes

- template classes, can not be instantiated, only extended
- use `abstract` keyword before `class` keyword
- can contain abstract properties without implementation, prepend `abstract` keyword
- can contain "concrete" properties with implementation
- abstract class is compiled to normal class, abstract properties are removed
- like an interface that is implemented, but unlike interface it is compiled to JS with the "concrete" properties
- like parent class with protected constructor, but can also contain abstract properties

```typescript
abstract class Animal {
  abstract eat(): void;
}

class Lion extends Animal {
  eat() { console.log("Feeding...") }
  growl() { console.log("Grrrr...") }
}

// Non-abstract class 'Goat' does not implement inherited abstract member 'eat' from class 'Animal'.(2515)
class Goat extends Animal {
  maeh() { console.log("Maehhh...") }
}

const x = new Animal(); // Cannot create an instance of an abstract class.
const y = new Lion();
```