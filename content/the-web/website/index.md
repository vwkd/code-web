---
title: Website
author: vwkd
index: 9
tags:
  - the-web
---

## Web page

- hypertext document (and associated resources) on the WWW, i.e. accessible via an URL
- written in client-side languages, e.g. HTML, CSS, JavaScript, and WebAssembly
- beware: web page view in browser can look completely different due to JavaScript
- beware: often used as synonym for website ❗️

### Build

- assembly of the web page
- can be static or dynamic, differs in time when web page is built
- beware: don't confuse time when built with interactivity, can always make interactive using client-side JavaScript ❗️
- beware: don't confuse web page with web page view in browser window, can always personalise using client-side JavaScript, e.g. AJAX to HTTP API, current time, location, etc. ❗️
<!-- todo: above should be "content" instead of "web page view" since web page view is defined by rendering?? -->
- beware: other resources of web page besides hypertext document are usually assumed to be static, but for dynamic web page could also generate resources on-the-fly, e.g. personalised images, style sheets, script files, etc. ❗️
- popular static site hosts include [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/), [GitHub Pages](https://pages.github.com/), [surge](https://surge.sh/), etc.
- beware: misnomer in today's times, since client-side JavaScript can make any web page "dynamic", interpret as "static / dynamic Web server" instead ❗️

#### Static web page

- built in advance
- stored on Web server assembled, e.g. file on file system
- Web server only needs to serve web page
- non-personalisable to request, e.g. user-specific data
- can use for web page that is the same for any two request at the same time
- advantages:
  - low latency, because no additional server-side processing
  - cachable, because always the same, e.g. can use CDN
  - scalable, because doesn't need custom Web server, e.g. can use static site host
  - secure, because Web server doesn't build web page, i.e. uncoupled from database
- source code can be only in client-side languages, e.g. HTML, CSS, JavaScript, etc.
- beware: source code doesn't need to be only in client-side languages, can use templates as well, see SSG (Static site generator) ❗️
- can appear more dynamic with regular builds, e.g. comments

#### Dynamic web page

- built on request on-the-fly
- stored on Web server disassembled, e.g. template file, database entry
- Web server needs to build and serve web page
- personalisable to request, e.g. user-specific data
- can use for web page that can differ for any two request at the same time, e.g. due to authentication
- can use for lots of identical web pages where only content differs, e.g. Wikipedia
- disadvantages:
  - high latency, because additional server-side processing, e.g. limited by database speed
  - not cachable, because not always the same, e.g. can't use CDN
  - not scalable, because needs custom Web server, e.g. can't use static site host
  - insecure, because Web server also builds web page, e.g. coupled to database
- source code must be additionally in server-side languages, e.g. PHP, etc.
- can appear more static with caching

### Rendering

- assembly of the view of the web page
- can be server-side or client-side, differs in location where view is built
- beware: misnomer, not the build of the graphical view, but the markup that generates it ⚠️
- beware: don't confuse view with content, can always personalise content using client-side JavaScript, e.g. AJAX to HTTP API, current time, location, etc. ❗️
- beware: don't confuse rendering with interactivity, can always make interactive using client-side JavaScript ❗️
- hydration: process of adding interactivity to web page through client-side JavaScript, e.g. attach event handlers, etc.

#### Server-side rendering

- built on server, by assembling HTML
- HTML contains view, semantic
- JavaScript contains only any interactivity
- can use for web page which is not performance critical
- advantages:
  - fast load, because can view already before any JavaScript loads
  - low bandwith, because doesn't need to transmit additional JavaScript runtime
  - energy efficient, because doesn't need additional client-side script execution
  - accessible, because view doesn't depend on JavaScript, e.g. good SEO
  - independent, because doesn't necessarily need JavaScript besides HTML and CSS
- web page can be static or dynamic
- beware: sometimes term is used for dynamic web page and called explicitly "Pre-rendering" / "Static rendering" for static web page, here we will use it only for static web page ⚠️
- usually web pages built with a (server-side routed) SSG, e.g. [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), Jekyll, etc.

#### Client-side rendering

- built on client, by manipulating DOM through JavaScript
- HTML contains empty skeleton, non-semantic
- JavaScript contains view and any interactivity
- can use for web page which is not performance critical, e.g. login page
- disadvantages:
  - slow load, because can view only after JavaScript loads
  - high bandwith, because needs to transmit additional JavaScript runtime
  - energy inefficient, because needs additional client-side script execution
  - inaccessible, because view depends on JavaScript, e.g. bad SEO
  - dependent, because necessarily needs JavaScript besides HTML and CSS
- web page can be static or dynamic
- beware: not used for dynamic websites usually since already generates HTML on-the-fly on server, but could also instead generate JavaScript on-the-fly ❗️
- usually web pages built with a client-side Web framework, e.g. Svelte, React, etc.



## Website

- set of web pages located under single domain name
- beware: often used as synonym for web page ❗️
- static / dynamic, when all web pages are static / dynamic
- client-side rendered / server-side rendered, when all web pages are client-side rendered / server-side rendered
- Single Page Application (SPA): web application with client-side routing, see Routing

### Routing

- navigation of pages of a website
- can be server-side or client-side, differs in location where pages are built
- beware: "pages" are not necessarily web pages, e.g. Client-side routing ❗️
- beware: don't confuse with "routing" in other contexts, e.g. on Web server ❗️

#### Server-side routing

- pages are web pages
- loaded through hyperlink
- full page reload
- for browser is new web page
- can use for document
- disadvantages:
  - high bandwith, because loads duplicate data, e.g. hypertext document, (other resources are probably cached), etc.
  - slow, because can't intelligently preload, e.g. on hover, on keydown (instead of keyup), etc.
  - ugly, because can't control visual transition, e.g. flashing white, no custom animation, etc.
  - inefficient, because can't continue same application state in memory, e.g. cache to disk and reload on next page
- advantages:
  - accessible, because browser knows it's a new page
    - accessibility works, e.g. focuses top, reads title, etc.
    - URL, history, back / forward buttons work out-of-the-box
    - analytics script works out-of-the-box
  - independent, because doesn't necessarily need JavaScript besides HTML and CSS
  - simple, because doesn't need additional HTTP API
- website can be server-side or client-side rendered
- beware: not used for client-side rendered website usually since already loaded a JavaScript runtime that could use for client-side routing ❗️
- usually web pages built with a (server-side routed) SSG, e.g. [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), Jekyll, etc.

#### Client-side routing

- pages are views
- loaded through AJAX to HTTP API
- partial DOM update
- for browser is same web page
- can use for application 
- advantages:
  - low bandwith, because loads only data that changed (apart from initial JavaScript runtime), e.g. JSON
  - fast, because can intelligently preload, e.g. on hover, on keydown (instead of keyup), etc.
  - beautiful, because can control visual transition, e.g. no flashing white, custom animation, etc.
  - efficient, because can continue same application state in memory
- disadvantages:
  - inaccessible, because browser doesn't know it's a new page
    - accessibility doesn't work, e.g. doesn't focus top, doesn't read title, etc.
    - URL, history, back / forward buttons need to be updated manually by JS, e.g. map URL uniquely to application state
    - analytics script needs to be manually informed
  - dependent, because necessarily needs JavaScript besides HTML and CSS
  - complex, because needs additional HTTP API
- on initial page load website can be server-side or client-side rendered, for server-side uses hydration
- after initial page load website can only be client-side rendered since pages are built on client
- usually web pages built with a client-side Web framework and a routing library, e.g. Svelte + Routify, React + React Router, etc.
- beware: needs to store static mirror of pages on server such that any page can be initial page
- usually web pages built with a client-side routed SSG for a client-side Web framework, e.g. Svelte + Sapper, React + Gatsby, etc.



## LAMP stack

- traditional stack of a website
- creates dynamic website, server-side rendered, server-side routed
- consists of Linux, Apache, MySQL / MariaDB, and Perl / PHP / Python
- beware: many variations using different operating systems, web servers, databases or programming languages, e.g. LAPP, LNMP, LLMP, LYME, LYCE, etc.
- makes website dynamic on server-side using Web server integrations
- server-side processes are confined to custom server, needs to manage underlying platform, e.g. dynamic Web server, database, authentication, etc.



## JAMstack

- modern stack of a website
- creates static website, server/client-side rendered, server/client-side routed
- consists of client-side JavaScript, reusable APIs, and prebuilt Markup
- makes website dynamic on client-side using AJAX to HTTP API
- beware: if data from HTTP API isn't personalised or real-time critical can fetch on server-side at build time instead of on client-side at run time,and make look dynamic with frequent builds, e.g. news, comments, etc. ❗️
- server-side processes are decoupled into microservices, can use third-party Cloud services instead of managing underlying platform, e.g. static Web server, database, authentication, etc.



## Resources

- [Dave Aglick - What Makes A Static Site?](https://daveaglick.com/posts/what-makes-a-static-site)
- [Mathias Biilmann - The New Front-end Stack. Javascript, APIs and Markup](https://vimeo.com/163522126)
- [Mathias Biillman - Rise of the JAMstack](https://www.youtube.com/watch?v=uWTMEDEPw8c)
- [Phil Hawksworth - Introduction to JAMstack](https://www.youtube.com/watch?v=A_l0qrPUJds)