---
title: Terminology
author: vwkd
index: 1
tags:
  - programming
---
# Terminology

<!-- todo: reorder better -->



## Computer

- machine that executes instructions
- consists of hardware and software



## Hardware

- physical parts of a computer



## Software

- instructions for a computer
- often used as synonym for program



## Program

- set of instructions that make computer perform a particular task
- written in a programming language
- can program another program, high-level program is input to low-level program, can think of high-level program as controller of low-level program, like the driver interface to a car (steering wheels, pedals, etc.)



## Compiler

- a low-level program that translates a high-level program to same low-level
- e.g. C to assembly
- can be ahead-of-time (AOT) or just-in-time (JIT)
- automates work for the high-level program, e.g. memory management, variable contexts, etc.



## Interpreter

- a low-level program that executes a high-level program
- contains a compiler, e.g. JavaScript to bytecode



## Runtime environment

- native app on OS with an interpreter
- provides access to OS, e.g. threads, OS APIs, etc.
- beware: sometimes shortened to "runtime", don't confuse with runtime as period of time ❗️



## Code

- instruction in a program
- statement: unit of code that performs some action, e.g. `x = 1 + 2;`
- expression: unit of code that evaluates to a value, e.g. `1 + 2`
- evaluation: computation of an expression
- block: list of statements enclosed in `{}`



## Application Programming Interface (API)

- particular set of rules that two software programs can follow to interact with each other
- abstraction layer of underlying implementation



## Software stack

- a hierarchical set of software that work together
- e.g. operating system, application (web server, database, etc.), programming language, etc.
- often named according to first letter of each member of set
- beware: often uses "stack", don't confuse with data type or call stack ❗️



## Compatibility

- backward compatibility: earlier specification works in future specification, e.g. HTML4 in environment for HTML5
- forward compatibility: future specification works in earlier specification, e.g. HTML5 in environment for HTML4



## Garbage collection

- runtime automatically frees memory when not needed anymore
- after runtime automatically allocated memory when needed
- reference: when an object has access to another, implicit or explicit, e.g. through closure, property, etc.
- -> still needs to write good code to not block garbage collection

### Reference-counting garbage collection

- object is garbage collected when there are no more references to it
- problem: circular references prevent garbage collection, common source of memory leaks

### Mark-and-sweep garbage collection

- object is garbage collected when there are no more references to it from the root (global object), when it is not "reachable" anymore
- solves cirular reference problem



## Operating System (OS)

- program that provides user programs access to hardware, e.g. memory, drives
- consists of kernel



## Process

- instance of a user program
- has address space, heap, global variables, open files, etc.
- doesn't share resources with other processes, isolated
- multitasking if multiple processes can be active at same time
- has one or more threads



## Thread

- sequence of "execution units" of a process
- has program counter, registers, call stack, etc.
- share resources with other threads of same process
- single CPU (core) can run one thread at a time, multiple CPU (cores) can run threads in parallel
- multithreading if OS can switch single CPU (core) between multiple threads, concurrency
- multiple threads can keep process responsive, because OS can switch to other threads when one thread blocks on a long-running task



## Operation

- block of code that's executed together
- runs uninterrupted start-to-end
- just order of operations can differ

## Synchronous

- operations in order
- one operation waits for another to finish before it starts
- "blocking"
- can depend on result of other operation, dependent
- "Synchronous programming is disrespectful and should not be employed in applications which are used by people." - Douglas Crockford
- execution is sequential

```text
--------------> time

sequential:
--a1--a2--b1--b2--
```

## Asynchronous

- operations out-of-order, interleaved
- one operation doesn't wait for another to finish before it starts
- "non-blocking"
- can't depend on result of other operation, independent
- beware: result would be non-deterministic if it depended on the other operation, because execution time is not deterministic, can't guarantee that one operation finishes before the other ⚠️
- execution is either concurrent (in same time frame) or parallel (at same time)

```text
--------------> time

concurrent:
--a1--b1--a2--b2--

parallel:
--a1--a2--
--b1--b2--
```



## Data structures

### Stack

- data structure where collection of elements follows Last In First Out (LIFO)
- like pile of plates
- has push and pop operations, e.g. array
- "stack overflow" when stack exceeds available memory, e.g. call stack for infinite (non-tail) recursion

### Queue

- data structure where collection of elements follows First in First Out (FIFO)
- like line at grocery store
- has enqueue and dequeue operations



## Subroutine

- reusable sequence of instructions
- "returns" to return address after done
- return address is address following the instruction that called subroutine
- function: subroutine that returns value
- procedure: subroutine that returns no value
- usually uses a call stack



## Call stack

- stack of active subroutines of program
- beware: often uses "stack", don't confuse with data type ❗️
- stack frame: element of call stack
- stack frame contains private data of subroutine call, e.g. return address, parameters, internal variables, etc.
- when subroutine is called creates stack frame (push on top of stack), when subroutine returns removes stack frame (pop from top of stack)
- "stack trace": log of call stack, can use to backtrace error



## Resources

- Wikipedia