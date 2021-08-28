---
title: Browser Object Model (BOM) API
author: vwkd
index: 2
tags:
  - languages
  - javascript
  - web-apis
---

- API to interact with a Web browser window
- not standardized, implementation can differ between browsers
- objects `window`, `console`, `navigator`, `history`, `screen`, `location`, etc.



## `window` object

- represents browser window (tab)
- main object, all other objects are properties of it
- methods: `open`, `prompt`, `alert`
- properties: `innerWidth`/`innerHeight` size of viewport, `outerWidth`/`outerHeight` size of browser window, `scrollX`/`Y` distance scrolled, etc.
- is global object, i.e. global `this`, can access its properties directly, no need to specify e.g. `window.console.log()`
- all function statements are methods of it, i.e. including built-in constructors like `String()`, `Math()`, `JSON()`, `Object()`, etc.
- all global variables, i.e. `var`, are properties of it
- all other Browser API objects are properties of it, i.e. `document`, `fetch`, `setTimeout()`, etc.
- beware: don't confuse properties and inheritance chain, being a property of an object and being in its inheritance chain are completely seperate things, i.e. `document` does not inherit from `window`, DOM does not inherit from BOM



## Resources

- MDN - as usual