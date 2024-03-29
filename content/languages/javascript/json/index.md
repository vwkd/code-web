---
title: JSON (JavaScript Object Notation)
author: vwkd
index: 12
tags:
  - languages
  - javascript
---

- format for text
- used to transmit data between different programs
- can represent all essential data types, i.e. number, boolean, string, array, object, null
- doesn't support any other data types, e.g. function, date, regular expression, and undefined etc. ❗
- file type `.json`, MIME type `application/json`



## Syntax

- follows JS literal syntax, e.g. string literal, array literal, object literal, etc.
- whitespace is ignored
- number: no leading zeros or trailing dots
- string: double quotation marks only, escape chars are allowed
- object: keys as strings, no methods since they are functions
- beware: only one missing comma makes JSON text invalid ❗️



## `JSON` object type

- `JSON.parse()`: converts string from JSON to data type, deserialisation
- `JSON.stringify()`: converts data type to JSON string, serialisation



## Resources