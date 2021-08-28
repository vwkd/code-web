---
title: Running JS
author: vwkd
index: 1.1
tags:
  - languages
  - javascript
---

## REPL (read–eval–print loop)

<!-- todo -->



## Types of client-side JS

- part of a Web page, sandboxed to that page
- file extension `.js`

### External 👍

- script in separate .js file
- reference .js in `<script>` element anywhere in `<html>` section

```html
<script src="...js"></script>
```

- can omit `type` attribute since JS is default scripting language
- content must be empty if `src` attribute is present
- best practice, separates JS from HTML

### Internal 👎

- script in .html file
- script in `<script>` element anywhere in `<html>` section

```html
<script>...</script>
```

- not recommended, mixes JS with HTML

### Inline 👎

- script in .html file
- script in on`event` attribute of single HTML element

```html
<tagname onevent="...">...</tagname>
```

- not recommended, mixes JS with HTML



## Loading external scripts

- when running a script, it always blocks the page, i.e. parsing during page load must be paused, can either do in between or after page load
- loading script can be done asynchronously parallel to page load or synchronously blocking page load
- by default `<script>` tag is (loaded and) executed at location in HTML
- problem: blocks parsing of page until finished, also can't access later page content because it's not yet loaded
- primitive solution: put script at end after `<body>` tag, but delays execution since browser can only start to load it after it parsed HTML until location
- modern solution: load scripts async, even multiple at same time, but for execution still needs to stop page load or wait until is done
  - `async`: fetches script async, executes it as soon as loaded, then blocking page, async scripts are executed in order of loading, what loads first runs first, good if script is independent from page and other scripts, e.g. ads, calculations, etc.
  - `defer`: fetches script async, executes it after page load, deferred scripts are executed in order of appearance, good if script is dependent on page
- by default any code is run in global scope, i.e. later variables can overwrite previous ones, has access to content of other scripts, i.e. use IIFE to limit scope
- a script is limited to current browser tab

### ES Modules

- problem: load multiple scripts, needs to be in exactly the right order they depend on
- primitive solution: concat to single file, needs to be in order they depend if dependencies are not all hoisted (e.g. const)
- still problem: clutter and modify global scope, browser probably loads lots of dead code on every page
- modern solution: ES Modules
  - only needs single script tag for entry file, order is figured out by browser 🎉
  - every module has it's own scope, i.e. solves global scope problem 🎉
  - browser can load only what's really needed on current page 🎉
- split in as many modules as possible
- use `type = "module"` on script tag because modules are parsed differently than classical scripts, further imported modules are known to be modules already
- module script is deferred by default, i.e. no need for `defered` attribute
- script tag with boolean attribute `nomodule` is ignored by module-supporting browsers, i.e. can provide fallback for non-module-supporting browsers

```html
<script type="module" src="main.js"></script>
<script nomodule src="fallback.js"></script>
```

- beware: use dynamic import for dependencies in module if are used only in certain condition, then use `<link rel="preload">` or show loading indicator ❗️