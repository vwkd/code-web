---
title: Protocols
author: vwkd
index: 1
tags:
  - the-web
---

- set of rules that define how data is being exchanged via a given medium
- defines data format, address format, transmission, reception, error handling, etc.
- protocols are to communication what programming languages are to computations
- needed especially to build networks, since can't control both ends of the communication
- a message is just a text file with agreed upon syntax according to protocol, usually uses ASCII encoding



## State

- can be stateful or stateless
- stateful: a message can rely on previous messages
- stateless: no message can rely on previous messages



## Layers

- protocol suite: set of protocols
- protocol layers: protocol suite whose protocols builds on top of each other
- put message of one protocol as data in the message of another protocol
- a layer provides service to the layer above it using the services of the layer below it
- used to separate different levels of abstraction



## Resources

- [Wikipedia - Communication Protocol](https://en.wikipedia.org/wiki/Communication_protocol)
