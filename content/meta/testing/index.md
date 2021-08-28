---
title: Testing
author: vwkd
index: 3
tags:
  - meta
---
# Testing



## Introduction

- assert expected outcome of implementation on sample data
- warns from regression, changes break existing features, can confidently make changes
- integrate in release pipeline, address bugs immediately, e.g. on every commit
- needs set of sample data, as close to real data as possible
- beware: never rely only on tests, can not cover every possible path of execution ❗️
- beware: assert a result, not how a result is made ❗️



## Test cases

- test all critical use cases, edge cases, etc.
- test both desired and undesired use cases, i.e. expected results and expected errors ❗️
- test exact result, e.g. not only "contains" but "contains exactly"



## Testing levels

- writing god tests is not trivial, bad tests lead to more maintanance
- beware: don't mix test levels, no "hybrid tests", loses assumption of independence ❗️

### Unit tests

- tests of underlying units of code, e.g. function, class, etc.
- test one unit with one test
- not multiple units in one test, no "hybrid tests", makes test dependent on multiple units
- not multiple test for one unit, just more maintanance without more code coverage
- no assumptions on codebase above, independent of what application features it is combined to, i.e. doesn't break if application features change due to different composition of units ❗️
- usually written by developer together with code

### Integration tests

- tests of overarching features of application
- no assumptions on codebase below, independent of what underyling units application is made of, i.e. doesn't break if underlying codebase changes but still resulting in the same application features ❗️



## Other tests

### Performance tests
<!-- ToDo: Finish -->

- benchmarks ???
- warns if changes impact performance, can make changes confidently



## Test-driven development

- write unit tests first, then implementation
- forces developer to think about desired result first, focuses development on important things
- documents intentation
- tests implementation