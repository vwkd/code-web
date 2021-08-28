---
title: Introduction
author: vwkd
index: 1
tags:
  - javascript
---

<!-- todo: finish -->
<!-- todo: remove (with ES6) notes, because are not complete at all
consider removing all ES-X notes, and refer just to places where can look up
also doesn't necessarily mean is not available, e.g. private class fields are for ES2022 but are available for a few years already
https://caniuse.com/
 -->
<!-- todo: check that all additions past ES2017 are included
https://github.com/tc39/proposals/blob/master/finished-proposals.md
https://2ality.com/2017/02/ecmascript-2018.html
https://2ality.com/2018/02/ecmascript-2019.html
https://2ality.com/2019/12/ecmascript-2020.html
https://2ality.com/2020/09/ecmascript-2021.html
 -->
<!-- todo: note that doesn't cover everything, just basics -->

## ECMAScript (ES)

- standard of language
- beware: doesn't contain Web APIs, i.e. doesn't have any side effects
- "JavaScript" is trademark of Oracle, has no relation with Java except historic name to attract popularity
- dynamic typing, prototype-based inheritance, first-class functions

### ECMAScript versions

| ES (ECMAScript) | year      |
| --------------- | --------- |
| ES1             | 1997      |
| ES2             | 1998      |
| ES3             | 1999      |
| ES4             | abandoned |
| ES5             | 2009      |
| ES6 / ES2015    | 2015      |
| ES2016          | 2016      |
| ES2017          | 2017      |
| ES2018          | 2018      |
| ES2019          | 2019      |
| ES2020          | 2020      |
| ES[year]        | [year]    |

- feature gap: gap of current ES version to ES version supported by runtime environment
- use transpiler to bridge feature gap, supporting old runtime environment shouldn't come at the expense of the developer, developer needs to be able to use contemporary features



## Implementation

- usually just-in-time compiled, first parsed (catches static errors like syntax, builds scope, etc.), then executed
- modern runtimes use Mark-and-sweep garbage collection, old ones used Reference-counting garbage collection



## Resources