---
title: Reactive Programming
author: vwkd
index: 3.2
tags:
  - programming
---
# Reactive Programming



## Introduction

- a declarative programming paradigm
- abstracts away time of computer, code specifies state of program for any point in time, not discrete temporal sequence of steps how its executed
- allows an application to update automatically on state change, e.g. interactive software
- often used together with functional programming, declarative operations + declarative time, functional reactive programming (FRP)
- beware: term is highly misused, e.g. by company promoting its cloud business of applications that scale dynamically (Reactive Manifesto) ❗️



## Motivation

- physical time is continuous, everything evolves continously in time, not a temporally discrete set of changes, e.g. position of a person, time itself, etc.
- a computer operates in a temporally discrete sequence of steps, ultimatively limited by clock cycle of CPU
- programming languages describe discrete steps of execution in time, imperative, designed for computer, e.g. each line of compiled code (assembly) can be thought of as one step, actually may be multiple but still discrete
- for a bare computation this is fine, just run code as sequence of steps to obtain result, can ignore time
- for complex applications not fine, need to keep running indefinitely and react to changes (side effects), need to have a notion of time to be able to react to time-varying values, e.g. show animation, process side effects like mouse clicks, mouse movement, key presses, user inputs, network requests, etc.
- implementation will always have to be discrete sequence of steps so computer can execute it one at a time, but programming language could be declarative
- ultimate declarative programming language would only describe _what_ application state for any point in time should look like, not temporally discrete steps _how_ to get to it, e.g. GUIs like spreadsheet formulas, a CSS animation rule, etc.
- can let application update its state automatically, "react" to time-varying values, just like spreadsheet, as soon as cell changes all dependent cells update their values automatically
- describe time-varying values declaratively, not discrete imperative steps in time, time of computer is abstracted away
- application time can even be continuous, needs to be discretised only when program is compiled into discrete sequence of steps for CPU, e.g. animation is continuous until output, then discretised to some number of FPS, or graphic is continuous in space, only rasterised to discrete pixels when output to screen
- a time-varying value is ultimately introduced by side effects into program, e.g. user input



## Theory

- needs first-class representation of time-varying values, not indirectly encoded in sequence of execution where history is lost
- needs continuous application time, not tied to discrete sequence of execution, resolution-independent
- only at implementation level application time is discretised upon converting program to discrete sequence of execution steps

### Behavior

- time-varying value, value that can change over time, e.g. mouse position, volume slider, text value in input field, etc.
- has access to complete time history, evolution of value
- value can be any value, e.g. boolean, string, object, etc.
- can think of as mathematical function from time to value, e.g. mouse position going in a circle with radius one around the origin of the coordinate system `f(t) = (sin(t), cos(t))`
- analogy of mathematial function breaks down if value is not a number, e.g. can't denote time-varying string, or time-varying array
- like a continuous stream of values for any point in time (like water, music, space etc., not like discrete event stream)
- non-changing values are still behaviors, just a constant function, e.g. number of continents `g(t) = 7`
- can compose with other behaviors to get new behavior, e.g. `h(t) = g(t) + f(t)`, add its value to itself from a shifted point in time `h(t) = g(t) + g(t+42)`, etc.
- can transform to get new behavior, using traditional function applied at every point in time, e.g. `toUpperCase(g(t))`, or shift time `h(t) = g(t+42)`, warp time `h(t) = g(t*2)`, etc.
- "reactive value", makes time first-class, replaces old way of thinking about values updated in discrete steps of execution, see Time for what lines of code then mean
- use behavior to define application state, e.g. `console.log(g(t))`
- can think of statements dependent on the value of a behavior as being defined _completely_ at time of declaration, updated automatically and "instantly" as soon as value of behavior changes, as if dependency were mathematical relationship, if one changes the other instantly changes, like entangled particles

### Event

- discrete stream of `(time, value)` tuples
- model discrete occurrences
- can think of as behaviors which are undefined except at discrete points in time, mathematical function with discrete set of points where its defined (domain)
- can compose with other event to get new event, interleaved in time, need to figure out what happens with two occurrences at same time
- can transform to get new event, using traditional function applied to every occurrence, e.g. filter out, manipulate, etc.
- beware: "event stream" would probably have been a more accurate term, don't confuse with normal "events" in programming languages

### Time

- distinguish "application time" from "computer time", in non-reactive language application time matches computer time
- application time is imagined to be continuous, application state is defined for all points in time
- computer time is discrete, ultimatively limited by clock cycle of CPU, often wants to limit much earlier by output medium, e.g. 60 Hz refresh rate of screen
- implementation needs to discretise application time, time steps need to be coarser than computer time steps, such that CPU can execute for each point in discrete application time all operations needed to determine application state
- code is written in application time
- since application time is continuous, lines of code don't correspond to discrete steps of execution in application time anymore, e.g. current line is present, lines above are past, lines below are future
- lines of code specify whole application state for any given point in application time, declarative rules instead of discrete sequence of execution, e.g. like CSS, HTML, etc.

```javascript
// pseudocode, imaginary reactive language

x = <mouse-x>;
y = x * 2;
console.log(y);
// x updates constantly as you move the mouse, any dependencies in turn as well, i.e. y and console.log(y)
```

- beware: at first it's hard to rethink what code means, doesn't correspond to sequential steps of execution in application time anymore, many example code snippets still use sequential lines to denote different points in application times ⚠️

```javascript
// pseudocode, imaginary reactive language

x = 1;
y = x * 2;
x = 21; // 👎 uses sequential lines of code to denote different points in application time
// y is 2 and not 42
```

- can imagine code is evaluated "instantly" for a given point in application time, or imagine application time is freezed while it is evaluated
- application is evaluated for `t = 0`, then keeps running in "loop" for continuously evolving application time until quit, at each point in application time behaviors are reevaluated and the application state recomputed, from point of view of application time updating the application state looks "instant"
- lines of code still correspond to discrete steps of execution in _computer time_, CPU can't just run operations "instantly", updating the application state takes some time, unlike entangled particles
- but can think of lines of code being run "instantly" from the point of view of application time, codes as if application time were continuous
- implementation needs to figure out how to discretise code into concrete sequence of execution steps in computer time, how to update application state in temporally discrete sequence of steps
- beware: nothing would prevent creating circularly dependent behaviors, however time-varying values can _not_ have circular dependencies, because in time there can't be circular dependencies without breaking causality ❗️

```javascript
// pseudocode, imaginary reactive language

x = y;
y = x;
// if y changes, so does x, so does y, so does x...
```



## Implementation

- provides abstractions for behavior, event, built-in transformation functions, etc.
- translates reactive code into discrete sequence of steps that computer can execute, figures out what code to run at what times
- beware: most implementations fall short of any real reactivity, as of May 2020 reactivity is not available in any mainstream programming language, only in Haskell with some libraries, e.g. Reactive-banana
- beware: "reactive" front-end frameworks only have the automatic application state update part, no behaviors with first-class representation, transformation, composition, and dependency chains, no events, e.g. React, Svelte
- beware: "reactive" event stream libraries just have events, no behaviors, e.g. ReactiveX
- beware: any implementations in traditional programming languages lack the syntax of making behaviors first-class values, need to work with existing non-reactive syntax, won't ever be truly reactive, needs new programming language

### Application state update

- discretise application time into discrete points at which can re-evaluate application state, choose points such that doesn't update when not necessary
- for each point in discrete application time, update only the parts that need to be updated to prevent unneccessary recomputation
- if application time is fine enough, makes the impression that application state is automatically updated as soon as value of behavior changes, application "reacts"
- needs to choose points such that doesn't evaluate value of behavior if didn't change, e.g. shouldn't be fixed by loop, should be variable by behavior that "notifies" when it changed
- needs to re-execute only the dependencies of behaviors that changed their value, needs to run chain of dependencies in correct order, "topological sorting"
- beware: examples below show only how lines of code don't correspond to sequential steps of execution in application time anymore, miss all the abstractions, also miss some concepts, e.g. sort dependencies in correct order

#### Idea 1: Loop

- run a constant timed loop
- in each iteration re-evaluate behaviors, if values changed re-execute dependent statements
- responsibility of dependencies to check if value of behavior they depend on changed, dependencies "pull" out the information, "tightly coupled" 👎
- unneccessary evaluation of behavior, behavior value is read even if didn't update 👎
- used by most reactive frameworks with loop interval set to match screen render rate, e.g. React, Svelte

```javascript
// --- Behaviors --- //

// use predefined functions here, but could be external variables that are changed by side effects (would need to be wraped as functions)

// time t in ms, error handling to asses input is valid time

// age increases by one each minute
function age(t) {
    if (typeof t == "number" && t >= 0) {
        return t / 60000;
    } else if (typeof t == "number" && t < 0) {
        return 0;
    } else {
        throw new Error("Invalid input.");
    }
}

function doubleAge(t) {
    if (typeof t == "number" && !Number.isNaN(t)) {
        return age(t) * 2;
    } else {
        throw new Error("Invalid input.");
    }
}

// --- Time --- //

// beware: each loop iteration is a point in application time, application time is "frozen", state update seems "immediate"

// define application time
let currentTime = 0;
const loopInterval = 1000;

// define cache values
let currentAge = age(currentTime);
let currentDoubleAge = doubleAge(currentTime);

// run loop async, such that external behaviors aren't blocked in mean time
function loop() {
    currentTime += loopInterval;

    // prevent unneccessary re-execution of dependencies
    if (age(currentTime) != currentAge) {
        // cache current value of behavior
        currentAge = age(currentTime);
        // re-evaluate dependencies of behavior
        console.log(`Your are ${age(currentTime).toFixed(2)} years old.`);
    }

    if (doubleAge(currentTime) != currentDoubleAge) {
        currentDoubleAge = doubleAge(currentTime);

        console.log(`Your are half-way there to ${doubleAge(currentTime).toFixed(2)} years.`);
    }
}

setInterval(loop, loopInterval);
```

#### Idea 2: Events

- (events in the usual sense)
- emit event when value of behavior changed, update dependencies in event handler
- responsibility of behavior to notify any dependencies if its value changed, behavior "pushes" the information, "loosely coupled" 👍
- no unneccessary evaluation of behavior, behavior value is read only if it updated 👍

```javascript
// --- Behaviors --- //

// use predefined functions here, but could be external variables that are changed by side effects (would need to be wraped as functions)

// time t in ms, error handling to asses input is valid time

// age increases by one each minute
function age(t) {
    if (typeof t == "number" && t >= 0) {
        return t / 60000;
    } else if (typeof t == "number" && t < 0) {
        return 0;
    } else {
        throw new Error("Invalid input.");
    }
}

function doubleAge(t) {
    if (typeof t == "number" && !Number.isNaN(t)) {
        return age(t) * 2;
    } else {
        throw new Error("Invalid input.");
    }
}

// --- Dependencies --- //

// use Events API of Browser, could have used Events API of Node as well, wouldn't have needed to bury data property
const ageEmitter = new EventTarget();

// one dependency on a behavior
ageEmitter.addEventListener("update", () => {
    console.log(`Your are ${age(currentTime).toFixed(2)} years old.`);
});

ageEmitter.addEventListener("update", () => {
    console.log(`Your are half-way there to ${doubleAge(currentTime).toFixed(2)} years.`);
});

// --- Time --- //

// beware: each loop iteration is a point in application time, application time is "frozen", state update seems "immediate"

// define application time
let currentTime = 0;
const loopInterval = 1000;

// needs loop to advance application time, also to keep script from quitting
function loop() {
    currentTime += loopInterval;

    // manually "change" behavior, but could be done outside of loop by side effects in mean time
    const updateAge = new CustomEvent("update");
    ageEmitter.dispatchEvent(updateAge);
}

setInterval(loop, loopInterval);
```



## Resources

- [Paul Stovell - What is Reactive Programming?](http://paulstovell.com/blog/reactive-programming)
- [Laurence Gonsalves - What is (functional) reactive programming?](https://stackoverflow.com/a/1028642/2607891)
- [Tikhon Jelvis - A Sensible Intro to FRP](https://begriffs.com/posts/2016-07-27-tikhon-on-frp.html)
- [Conal Elliott - The Essence and Origins of Functional Reactive Programming](https://www.youtube.com/watch?v=j3Q32brCUAI) (additional resources on [GitHub](https://github.com/conal/talk-2015-essence-and-origins-of-frp))
- [Conal Elliott - What is (functional) reactive programming?](https://stackoverflow.com/a/1030631/2607891)