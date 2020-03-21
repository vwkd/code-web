# Node

[TOC]



## Node

- JavaScript runtime, outside of browser
- cross-platform, device-independent
- many APIs like in browser, e.g. asynchronous, event-driven
- → can use JS to easily build cross-platform scripts / applications, instead of bash
- → rich ecosystem of packages, open source community



## Terminology

### Modules

- `.js` file that exports a function, can be imported in another `.js`, e.g. using `require()` in Node.js
- can be package but doesn't need to

### Packages

- folder containing a `package.json` and a `.js` file that is referenced as `main` property in `package.json`, by default is called `index.js`
- packages are modules, but modules are not necessarily packages
- device-independent packages, need to be coded properly using built-in modules, e.g. `path.join()` instead of string cocatening file paths