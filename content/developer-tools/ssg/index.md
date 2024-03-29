---
title: Static site generator (SSG)
author: vwkd
index: 2
tags:
  - developer-tools
---

<!-- ToDo: finish -->

- HTML template compiler
- creates static server-side rendered website
- website can be server-side routed, e.g. [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), Jekyll, etc.
- website can be client-side routed, e.g. Sapper, Gatsby, Next, Nuxt, VuePress, etc.



## Features

### Server-side routed website

- HTML template language
- markdown templates
- basic data flow, e.g. variables in frontmatter
- configurable routes
- set of pages, e.g. navigation, tags, etc.
- processing plugins, e.g. `<code>` syntax highlighting, etc.
- incremental build of only pages that changed
- etc.

beware: use HTML template language only for content output not for content generation, generate content programmatically and load in to processing cascade, e.g. breadcrumbs, etc. ❗️

### Client-side routed website

- server-side rendered pages for initial page load, i.e. has all above features
- code-splitting for each route
<!-- vvv basically a client-side router -->
- route preloading, e.g. on hover / keydown over hyperlink instead of on keyup
- route accessibility, e.g. history, focus on top of page, etc.
<!-- ^^^ -->

?? dynamic imports, changes module-wide import to dynamic import at place where it's used ???

?? optimisations
  preload of fonts, background-images, urls() in general
  preload dynamic imports



## Resources

- [Jamstack - Site Site Generators](https://jamstack.org/generators/)