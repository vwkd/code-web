---
title: Introduction
author: vwkd
index: 1
tags:
  - web-frameworks
---

<!-- ToDo: finish -->

## Client-side

- framework for web page, e.g. Svelte, React, etc.
- abstracts away underlying HTML, CSS and JavaScript, e.g. DOM API
- define structure, presentation and behavior in reusable and composable components instead of keeping them in separate files
- declarative updating of view when data changes, "reactive"
- compiles to JavaScript, i.e. web page is client-side rendered

### Site generator

- framework for website, e.g. SvelteKit, Next.js, etc.
- each web page is using the client-side web framework
- web pages can be defined filesystem-based, component-based, configuration-based, etc.
- can compile web pages for server-side rendering, server-side routing
- can include a router in server-side rendered pages for subsequent client-side routing, needs server endpoints for the data
- generates JavaScript for server endpoints of JavaScript-based HTTP servers, e.g. Node, Cloudflare Workers, etc.
- single code base for both front- and back-end of website
- beware: not official name but own invention, derived from "static site generator" ❗️



## Server-side

- framework for HTTP server, e.g. Koa, Express, etc.
- abstracts away underlying HTTP server, e.g. `http` module in Node
- exposes an API to declaratively handle requests and responses



## Resources