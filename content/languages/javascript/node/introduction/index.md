---
title: Introduction
author: vwkd
index: 1
tags:
  - node
---
# Introduction



## Node

- JavaScript runtime, outside of browser
- cross-platform, device-independent
- asynchronous, event-driven
- many APIs like in browser, e.g. `console`
- → can use JS to easily build cross-platform scripts / applications, instead of bash
- → rich ecosystem of packages, open source community



## Modules

- a `.js` file
- can be imported in another `.js` file using `require()`
- can export values that are then imported, e.g. object, function, class, etc.
- can be package but doesn't need to



## Packages

- a `package.json` and a module that is referenced as `main` property in `package.json`, by default is called `index.js`
- packages are modules, but modules are not necessarily packages
- device-independent packages, need to be coded properly using built-in modules, e.g. `path.join()` instead of string cocatening file paths ❗️



## Resources