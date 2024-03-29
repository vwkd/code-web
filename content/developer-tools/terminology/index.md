---
title: Terminology
author: vwkd
index: 1
tags:
  - developer-tools
---

## Compiler

translates source code from a high-level programming language to a lower level language, e.g. C to assembly



## Transpiler

- translates source code of one high-level programming language into another high-level programming language, e.g. CoffeeScript / Dart / TypeScript to JavaScript
- target programming languages may be the same as source, e.g. JavaScript to JavaScript
Babel
- used to write in desired language and compile back to undesired, e.g. TypeScript to JavaScript, or ES6+ to ES5



## (Module) Bundler

- concatenates separate files into single file
- orders by dependency, tree-shaking unused code
- beware: not necessarily needed with ES modules, just for tree shaking ❗️
- often integrates a build tool
- e.g. Rollup, Webpack, Browserify, etc.
- advantages:
  - single HTTP request
- disadvantages:
  - not cacheable cross-site



## Build tool

- converts source files to distributable files
- integrates compiler, transpiler, bundler, etc.
- integrates other optimisations, e.g. linting, minifying, sourcemaps, etc.
- integrates local server
- e.g. Snowpack, also Rollup, Webpack, etc.



## Task runner

- run any configurable task, not only building, e.g. linting, running tests, deploying, etc.
- e.g. gulp.js, browser-sync



## Resources