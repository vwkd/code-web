---
title: Workflow
author: vwkd
index: 3
tags:
  - developer-tools
---

<!-- ToDo: finish -->

- can use a script to call a build tool
- can have different scripts for development and production
- can run build automatically on file change ("watch")



## Example Build Process

- lint
- compile, e.g. TypeScript to JavaScript
- tree-shake, e.g. remove unused CSS / JS
- transpile, e.g. backport CSS / JS, autoprefix CSS
- [in dev build] optimisations, e.g. sourcemap
- [in prod build] optimisations, e.g. minify, uglify
<!-- todo: minify, uglify automatically done on static site host like Netlify? -->
- [in dev build] serve to localhost



## Example Deployment Process

- commit to VCS
- runs tests
- deploys automatically



## Resources