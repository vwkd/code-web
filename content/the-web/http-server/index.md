---
title: HTTP Server
author: vwkd
index: 7
tags:
  - the-web
---

- handle requests from client and send appropriate responses
- basically a complex series of if statements to figure out what to do for any given request



## Basic Tasks

- parse request headers, e.g. accepted content type, compression, etc.
- parse request body, e.g. form data, etc.
- perform input validation, e.g. escape HTML, etc.
- choose different response based on file path and method (routing), e.g. `index.html` for `/`, else `404.html`, etc.
- set response status code, e.g. `200 OK` or `404 Not Found` etc.
- set response headers, e.g. content type, content length, compression, etc.
- error handling and logging
- support state-of-the-art HTTP protocols, e.g. `Connection: keep-alive` header
- support security headers, e.g. ???
- etc. pp.



## Static server

- build response from local files, e.g. `index.html`, `style.css`, `script.js`, etc.
- needs to only support `GET` since data doesn't change



## Dynamic server

- build response from template and database
- keep session state, e.g. using cookies
- handle login authorisation



## HTTPS Server

- interfaces with underlying TLS implementation
- needs to provide certificate for identification
- use 301 redirect from HTTP to HTTPS version
- beware: always use HTTPS ❗️



## Resources