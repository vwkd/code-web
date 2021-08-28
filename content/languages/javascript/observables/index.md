---
title: Observables
author: vwkd
index: 13
tags:
  - javascript
---
# Observables

<!-- ToDo: Revisit if included by spec -->



## Introduction

- a better Events API
- beware: as of May 2020 not part of official spec, need to use third-party library, implementations might vary, e.g. ReactiveX with RxJS ⚠️
- beware: often confused with reactive programming, not more reactive than events are reactive, see [Reactive Programming]() for actual reactive programming



## Motivation

### Data flow

- consumer: consumes value(s), current code, always point of view
- producer: produces value(s), usually external side effect, e.g. Browser API
- data always flows from producer to consumer
- pull-based data flow: consumer is in control, consumer decides when to pull out a value from the producer, producer need to handle the waiting, synchronous
- push-based data flow: producer is in control, producer decides when to push out a value to the consumer, consumer need to handle the waiting, asynchronous
- beware: pull is sync and push is async from point of view of consumer, not necessarily same for producer ❗️

### Data flow types

- language implements different features to handle data flow
- differ in who has control and if data is single value or multiple values

| feature | single | multiple |
| - | - | - |
| pull | Function | Iterator |
| push | Promise | Event |

- Functions enable pull-based data flow with a single value, variable assignment pulls it out
- Iterators enable pull-based data flow with multiple values, iteration pulls it out
- Promises enable push-based data flow with a single value, pushed to handler function
- Events enable push-based data flow with multiple values, pushed to handler function
- Events lack several important things, which Promises and also Iterator have:
  1. producer can't notify consumer of error or completion, e.g. no error or completion callback in `.addEventListener()`
  2. producer starts pushing values immediately, doesn't wait for consumer to subscribe, e.g. can dispatch event even if nobody listens
  3. producer doesn't start again for new subscriber, pushes identical values to all subscribers at each point in time, late subscribers miss the values from the beginning



## Observable

- better Events API, fixes the above problems
- represents event stream as first-class object
- can apply declarative array-like transformations over it
- like multi-valued promise, promise that can resolve multiple times, with the support of 1., 2. and 3.
- push-based complement to pull-based Iterator, with the support of 1.
- replaces Event in above diagram

| feature | single | multiple |
| - | - | - |
| pull | Function | Iterator |
| push | Promise | Observable |

- two types of observables
  - cold: like multi-valued promise, i.e. like described above, fixes problem 1., 2., and 3. 👍
  - hot: like events with problem 2. and 3., i.e. just fixes 1. and adds the array-like operators 👎
- observer and operators can't control backpressure, e.g. `zip()` or `concatAll()` with one observer faster than the other 👎



## Implementation

<!-- ToDo: Revisit if RxJS changed -->

- beware: will use RxJS because most popular as of May 2020, but is not standard, like any library can change at any time ⚠️
- RxJS observables are cold, see subjects for hot ones 👍
- RxJS observables don't use microtask queue, are sync by default instead of async, need to manually make async 👎 ⚠️

```javascript
import { Observable } from "rxjs";

// sync by default
const obs = new Observable(subscriber => {
    subscriber.next(21);
    subscriber.next(42);
});

console.log("first");
obs.subscribe(x => {
    console.log(x);
});
console.log("last");

// first
// 21
// 42
// last
```

```javascript
import { Observable } from "rxjs";

// async by wrapping in Promise.resolve().then(..) or setTimeout(.., 0)
const obs = new Observable(subscriber => {
    Promise.resolve().then(() => {
        subscriber.next(21);
    });
    Promise.resolve().then(() => {
        subscriber.next(42);
    });
});

console.log("first");
obs.subscribe(x => {
    console.log(x);
});
console.log("last");

// first
// last
// 21
// 42
```

### Observing an observable

- observer / subscriber: event handler function(s), `next()`, `error()` and `complete()` handlers, analogous to event handler but with error and complete handler, can be three separate callbacks or object containing these methods, consumer
- observable: instance of `Observable` class, analogous to `EventTarget`, producer
- `Observable.subscribe()`: subscribes an observer, analogous to `EventTarget.addEventListener()`, returns subscription
- subscription: instance of `Subscription` class, represents a specific execution of an observable, no analog with events
- `Subscription.unsubscribe()`: unsubscribes an observer, analogous to `removeEventListener()`, beware: unlike `removeEventListener()` is not on event target instance but on return value of `addEventListener()` ❗️
- beware: "subscribe(r)" and "observe(r)" are used synonymously
- if no error handler is provided an error will be thrown normally
- the complete handler is only called when the producer completes the stream, not when the consumer calls `unsubscribe()`
- many built-in methods to create observables, e.g. `of()`, `from()`, `interval()`, etc.

```javascript
// from arguments
import { of } from "rxjs";

const observable = of(1, 2, 3);

const subscription = observable.subscribe(
  next => console.log(next),
  err => console.log('Ups!', err),
  () => console.log('Done!'),
);

// 1
// 2
// 3
// Done!
```

```javascript
// from array, iterable, string, promise, etc.
import { from } from "rxjs";

const observable = from([1, 2, 3]);

const subscription = observable.subscribe(console.log);

// 1
// 2
// 3
```

```javascript
// from event stream
import { fromEvent } from "rxjs";

const btn = document.getElementById("btn");

const observable = fromEvent(btn, "click");

const subscription = observable.subscribe(console.log);

// <any events>
```

beware: most methods create sync observables, only `fromEvent()` creates an async one ❗️

```javascript
// from arguments
import { of } from "rxjs";

const observable = of(1, 2, 3);

console.log("first");
const subscription = observable.subscribe(console.log);
console.log("last");

// first
// 1
// 2
// 3
// last
```

```javascript
// from array, iterable, string, promise, etc.
import { from } from "rxjs";

const observable = from([1, 2, 3]);

console.log("first");
const subscription = observable.subscribe(console.log);
console.log("last");

// first
// 1
// 2
// 3
// last
```

```javascript
// from event stream
import { fromEvent } from "rxjs";

const btn = document.getElementById("btn");

const observable = fromEvent(btn, "click");

console.log("first");
const subscription = observable.subscribe(console.log);
console.log("last");

// first
// last
// MouseEvent {..}
// ..
```

### Creating an observable

- can create observable manually instead of with built-in function from array, event stream, etc.
- `Observable` constructor takes a "subscribe" function,
- is passed an "observer" argument, can call `next(val)`, `error(err)` and `complete()` methods on observer argument
- "subscribe" function is called when `Observable.subscribe()` is called with observer, `next()`, `error()` and `complete()` call the equivalent handler functions on observer, beware: functions work the same to API user but separate in implementation ❗️
- observable contract: one or more `next()` calls followed by zero or one `error()` / `complete()` call, streams ends after first `error()` or `complete()` call, i.e. emitting an error means completion handler won't be called ❗️

```javascript
import { Observable } from "rxjs";

const observable = new Observable(function subscribe(observer) {
    observer.next(21);
    observer.next(42);
});
```

- wrap event notifications in try/catch block and catch exception with `error()` call

```javascript
import { Observable } from "rxjs";

const observable = new Observable(function subscribe(observer) {
    try {
        observer.next(21);
        observer.next(42);
        observer.complete();
    } catch (e) {
        observer.error(e);
    }
});
```

- beware: make event notifications async by wrapping in `Promise.resolve().then(..)` or `setTimeout(.., 0)` ⚠️
- "subscribe" function returns "unsubscribe" function, stop event stream and clean up any resources that observable execution used, e.g. stop `setInterval()`
- "unsubscribe" function is called when `Subscription.unsubscribe()` is called, beware: functions work the same to API user but separate in implementation ❗️

```javascript
import { Observable } from "rxjs";

const observable = new Observable(function subscribe(observer) {
    const id = setInterval(() => {
        observer.next(42);
    }, 1000);

    return function unsubscribe() {
        clearInterval(id);
    };
});
```

- the "subscribe" function is executed for each observer subscribing to an observable, guarantees separate observable execution for each observer, makes observable cold
  - observable doesn't start production until observer subscribes, lazy producer, fixes problem 2., i.e. no events are emiteed while nobody is subscribed
  - separate production for each observer, fixes problem 3., i.e. new observer can subscribe to observable at any time, even if it finished for a previous observer
- beware: events instead are eager producer, start production even before attaches handler, have only one production execution, returns result only to listeners currently registered, can miss value ❗️
- beware: promises instead are eager producer, start production even before attaches handler, have only one production execution, returns result to each handler, can't miss value ❗️



## Operators

- transform or compose event stream, e.g. merge, filter, map, delay
- can think of event stream as array over time, operator is applied when event passes through in present time
- operators are analogous to array functions
- often visualised using marble diagram
- operators are pure functions, return new observable
- applied using `.pipe()`, returns new observable, can chain multiple operators, analogous to `then()` handler for promises
- can think of as promise chain through which multiple values travel one after another, or as sequence of array transformations

```javascript
import { of } from "rxjs";
import { map, filter } from "rxjs/operators";

const observable = of(1, 2, 3, 4, 5);

observable
    .pipe(map(x => x * x))
    .pipe(filter(x => x % 2 == 0))
    .subscribe(console.log);

// 4
// 16
```

- can use multiple arguments shorthand `obs.pipe(op1, op2)` instead of `obs.pipe(op1).pipe(op2)`

```javascript
import { of } from "rxjs";
import { map, filter } from 'rxjs/operators';

const observable = of(1, 2, 3, 4, 5);

observable.pipe(
    map(x => x * x),
    filter(x => x % 2 == 0)
    ).subscribe(console.log);

// 4
// 16
```

- `.pipe()` manages subscriptions between chain, subscribing to output starts stream
- can use built-in `pipe()` function to create an operator from an operator chain

```javascript
import { of, pipe } from "rxjs";
import { map, filter } from "rxjs/operators";

const observable = of(1, 2, 3, 4, 5);

function squareEven() {
    return pipe(
        map(x => x * x),
        filter(x => x % 2 == 0)
    );
}

observable.pipe(squareEven()).subscribe(console.log);

// 4
// 16
```



### Subject

- makes observable hot, with problems 2. and 3., like events with array-like operators
- same observable execution for each observer, multicast an observable to multiple observers
- maintains registry of observers, like `EventTarget`
- acts as observable and observer, has necessary methods `.subscribe()`, `.next()`, `.error()`, `.complete()`, etc.
- observers subscribe to it, it then subscribes to observable, like intermediary broadcast station
- beware: not a first-class observable, can't customise using constructor function, don't use as primary observable ❗️

```javascript
import { Subject, from } from "rxjs";

const subject = new Subject();

const observable = from([1, 2, 3]);

subject.subscribe({
    next: v => console.log(`observerA: ${v}`)
});
subject.subscribe({
    next: v => console.log(`observerB: ${v}`)
});

observable.subscribe(subject);

// observerA: 1
// observerB: 1
// observerA: 2
// observerB: 2
// observerA: 3
// observerB: 3
```

- beware: subscribe first observers to subject, then subject to observable, otherwise observer miss values because transmission started before observers subscribed ❗️

```javascript
import { Subject, from } from "rxjs";

const subject = new Subject();

const observable = from([1, 2, 3]);

observable.subscribe(subject);

subject.subscribe({
    next: v => console.log(`observerA: ${v}`)
});
subject.subscribe({
    next: v => console.log(`observerB: ${v}`)
});
```

- can use some other types
  - `BehaviorSubject`: buffers current latest value, new observer get the current value pushed upon subscription, must give initial value
  - `ReplaySubject`: buffers last `n` values, can restrict to last `x` ms
  - `AsyncSubject`: emits only last value before completion of observable



## Resources

- [ReactiveX](http://reactivex.io/), [RxJS](https://rxjs.dev/)
- [ReactiveX - Operators](http://reactivex.io/documentation/operators.html), [RxJs Marbles](https://rxmarbles.com/)