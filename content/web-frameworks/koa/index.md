---
title: Koa
author: vwkd
index: 3
tags:
  - web-frameworks
---

- web framework
- by team behind Express, close to Express syntax
- minimal, doesn't bundle any middleware, e.g. router
- modern and expressive, uses async functions, better middleware pipeline flow, better error handling



## Hello World Example

```javascript
const Koa = require('koa');
const app = new Koa();
const PORT = 3000;

app.use(async ctx => {
  ctx.body = 'Hello World';
});

// sugar for http.createServer(app.callback()).listen(...) from http module
app.listen(PORT, () => {
  console.log(`Listening on ${PORT}...`);
});
```

```plaintext
GET http://localhost:3000/ HTTP/1.1
```

```plaintext
HTTP/1.1 200 OK
Connection: keep-alive
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Apr 2020 20:29:05 GMT
Content-Length: 12

Hello World!
```



## Request-response cycle

- each request is handed through a processing pipeline
- pipeline can modify request and response
- pipeline consists of multiple handler functions, see middleware function below
- at the end of the pipeline a response is always sent, i.e. server never ignores a request ❗️
- minimal response headers are set by default for all responses, e.g. connection, date, etc.
- minimal headers are set by default depending on response body, e.g. content-type, content-length, charset, etc.
- if no response status code is set, it is set automatically depending on the outcome of the pipeline
  - if response body was set, status code is set to `200 OK` (or `204 No content` if response body was set to `null`)
  - else, status code is set to `404 Not Found`, body is set to `Not Found`

```plaintext
HTTP/1.1 404 Not Found
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Apr 2020 20:26:22 GMT
Connection: keep-alive
Content-Length: 9

Not Found
```

- a `404` is not an application error, it's just the default response if pipeline didn't handle request
- most often response automatically is set to correct status code and headers, can always overwrite by setting manually



## Middleware function

- a handler function in the request-response processing pipeline
- registered with `app.use()`, is an async function
- gets called with with two arguments, usually called `ctx` and `next`
- can access request and reponse via the `ctx` object, e.g. add response body, etc.

```javascript
app.use(async ctx => {
  ctx.response.body = "Hello World";
});
```

- can call next middleware function via `next()`, next in order of registration, beware: must `await` since it's an async function
- pipeline is a stack-like sequence of middleware functions, cascades downwards and the back up, like recursion

```javascript
app.use(async (ctx, next) => {
  console.log("First");
  await next();
  console.log("Sixth");
});

app.use(async (ctx, next) => {
  console.log("Second");
  await next();
  console.log("Fifth");
});

app.use(async (ctx, next) => {
  console.log("Third");
  ctx.response.body = "Hello World!";
  console.log("Fourth");
});

app.use(async (ctx, next) => {
  console.log("Never reached.");
  // since previous middleware function didn't call next()
});
```

- beware: if doesn't call `next`, then the rest of the middleware stack is skipped, and since no response was built a 404 will be used ❗️
- in last middleware function `next()` will be a dummy function, i.e. last middleware function should call `next` as well for future proving ❗️
- beware: since pipeline is stack, needs to add handler for end at the beginning and yield first to `next()` such that stack unwinds at last back here, instead of adding handler to end of stack, e.g. error handler must go to beginning❗️

```javascript
// error handler
app.use(async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    console.log("Ups, got an error!", err);
    throw err; // rethrow because doesn't yet know how to handle
  }
});

// parsing handler
app.use(async (ctx, next) => {
  console.log("Parsing the request...");
  await next();
});

// routing handler
app.use(async (ctx, next) => {
  ctx.response.body = "Hello World!";
});
```

- good practice to separate middleware functions out into own module, e.g. route handlers in `routes` folder, etc.



## Context

- created for each request
- first argument of each middleware function, usually called `ctx`
- contains request and response objects `ctx.request` and `ctx.response`
- provides shorthands for all properties on request and response object, don't interfere since property names are all distinct, e.g. `ctx.body` is shorthand for `ctx.response.body`, etc.

```javascript
app.use(async ctx => {
  ctx.body = "Hello World";
});
```

- use `ctx.state` property to hold custom state for request, e.g. user data, etc.
- use `ctx.response.redirect()` to redirect response, if status code is not already set to a redirect status code uses `302 Found`, body is set to generic notice, can overwrite afterwards

```plaintext
HTTP/1.1 302 Found
Connection: keep-alive
Date: Sun, 26 Apr 2020 21:01:14 GMT
Location: /newurl
Content-Type: text/html; charset=utf-8
Content-Length: 43

Redirecting to <a href="/newurl">/newurl</a>.
```

- beware: await any promises in middleware function that modify the context, otherwise will send response before promise resolved, e.g. the following is always a `404`

```javascript
app.use(async ctx => {
  promiseReturningFunction().then(data => {
    ctx.body = data;
  })
})
```



## Error handling

- need to handle errors thrown in middleware functions, even deeper in API of third-party middleware
- if an error is thrown in middleware function, rest of stack is skipped, since async function stops and rejects the promise it returned
- if third-party middleware is properly coded, error bubbles up until top, since outer async functions just rethrow error
- beware: error handling deals with errors thrown, not a response with status code `404` because it wasn't handled by any middleware function ❗️

### Default error handler

- default error handler is like middleware function at very _top_ of stack, has a try-catch statement, calls `next()` in try statement, builds response in catch statement in case of error

```javascript
// error handler
app.use(async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    // handle error...
  }
});

// more middleware below
```

- response status code is set to `err.status`, else defaults to `500`
- response headers is set to `err.headers`, else defaults to minimal headers, i.e. clears any previously set headers
- response body is set to `err.message` if `err.expose` is true, else defaults to the status message that matches the response status code, e.g. `Internal Server Error` for `500`, i.e. doesn't leak information to client ❗️

```plaintext
HTTP/1.1 500 Internal Server Error
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Apr 2020 20:31:40 GMT
Connection: keep-alive
Content-Length: 21

Internal Server Error
```

- errors thrown normally (with `throw new Error()`) don't have a `status`, `headers`, or `expose` property, just a `message` property, i.e. `err.message` is not leaked to client, response is a generic `500 Internal Server Error`, see Throwing errors below how to throw errors with properties set using `ctx.throw()` ❗️

### Error listener

- `app` inherits from EventEmitter
- default error handler emits "error" event for every error
- can attach listener for "error" event, e.g. for central error logging

```javascript
app.on('error', (err, ctx) => {
  console.error('server error', err, ctx)
});
```

- if no listener is attached, default listener is used, otherwise Node process would exit because of unhandled error event, see Events
- default listener logs to `console.error()` except if `app.silent` is `true`, also not if `err.expose` is `true` or `err.status` is `404` (for `ctx.throw()`, actually `err.status = 404` check is redundant, since `err.expose = true` for `n < 500`)

### Custom error handler

- can add custom error handler as first middleware function to top of stack, i.e. right below default error handler, e.g. make response JSON, make response use HTML templates, etc.
- default error handler still catches any error after custom error handler, i.e. in custom error handler itself
- beware: don't blindly set response body to `err.message`, could leak sensible information about server to client, e.g. stack trace, might enable targeted attack on server, always check for `err.expose` before exposing `err.message`, copy default error handler ⚠️ ⚠️ ⚠️
- beware: if doesn't rethrow error, needs to emit "error" event manually to notify any error listener, since error doesn't reach default error handler anymore ❗️

```javascript
import statuses from "statuses";

// custom error handler function
async function handleError(ctx, next) {
  try {
    await next();
  } catch (err) {
    // normal throws don't have a status set, just for ctx.throw()
    ctx.status =
      typeof err.status === "number" && statuses[err.status] ? err.status : 500;

    // don't leak err.message, just for ctx.throw(n, message) with n < 500
    const msg = err.expose ? err.message : statuses.message[ctx.status];

    // manually notify error listeners
    ctx.app.emit("error", err, ctx);

    // build response
    ctx.type = "application/json";
    ctx.body = {
      message: msg,
    };
  }
}

app.use(handleError);
```

### Throwing errors

- `ctx.throw()` throws an error with properties `status`, `message` and `expose`, `expose` is set to `true` if `status < 500`, if `status` wasn't provided it defaults to `500`, i.e. is leak safe by default
- throw client errors with `status` at `4xx`, error message is exposed to client, e.g. `username not found`, etc.
- throw server errors with `status` at `5xx`, error message is not exposed to client, e.g. `can't read disk`, etc.
- can throw normally as well, since doesn't have `status` and `expose` property, a generic `500 Internal Server Error` is sent to client, i.e. error message is not exposed to client
- beware: don't set the properties of normally thrown error manually, hard to maintain and bug-prone, always use `ctx.throw()` ❗️
- beware: leak-free error handling requires that any custom error handler is properly configured, see Custom error handler ❗️
- use `ctx.assert()` for conditional `ctx.throw()`, e.g. `ctx.assert(ctx.accepts('json'), 406);`



## Routing

- select response based on endpoint (path and method) the request was sent to
- named in analogy to packet routing in networking, like selecting the path the request takes on the server

### Router

- just another middleware function

```javascript
import Router from "@koa/router";
const router = new Router();

app.use(router.routes()).use(router.allowedMethods());
```

- allows to register handler functions for particular routes, methods correspond to HTTP methods, first argument is path, e.g. `router.get()`, `router.post()`, `router.all()`, etc.
- calls handler function of _first_ route that matches endpoint of request, i.e. can provide default route at end to show a 404

```javascript
router.get("/hello", helloHandler);

router.get("*", notFoundHandler);
```

- boils down to if statement that checks which route matches the endpoint of the request

```javascript
// primitive router
app.use(async (ctx, next) => {
  if (ctx.path === "/hello" && ctx.method === "GET") {
    helloHandler(ctx);
  } else if (ctx.path === "/world" && ctx.method === "GET") {
    worldHandler(ctx);
  } else {
    notFoundHandler(ctx);
  }
});
```

- if no route matches, the router does nothing, as if router wasn't there, the application default is used at end, see Request-response cycle, i.e. simple 404

- can redirect URL for all methods using `router.redirect()`, `router.redirect(source, destination, [code])` is shorthand for

```javascript
router.all(source, ctx => {
  ctx.redirect(destination);
  ctx.status = code || 301;
});
```

### Multiple handler functions

- can register multiple handler functions for a particular route, only first one is called, can call next via `next()`, e.g. for conditional routes
- handler functions behave like stack, cascades downwards and back up, like in request-response pipeline, can think of handler functions as middleware functions inside a middleware function ❗️

```javascript
router.get(
  "/hello",
  async (ctx, next) => {
    console.log("First");
    await next();
    console.log("Fourth");
  },
  async (ctx, next) => {
    console.log("Second");
    ctx.body = "Hello World";
    console.log("Third");
  }
);
```

- beware: `next()` calls next handler function of any matching route, only call `next` if knows what next middleware function is, not just always like in request-response pipeline, i.e. default route at end matches everything, or `router.all()` method for same path ❗️

```javascript
router.get("/hello", async (ctx, next) => {
  console.log("First");
  await next();
  console.log("Fourth");
});

router.get("*", async (ctx, next) => {
  console.log("Second");
  ctx.body = "Hello World";
  console.log("Third");
});
```

### Route paths

- can be string, string pattern, or regular expressions, beware: `.` and `-` are interpreted literally ❗️
- can capture variable path using route parameter, parameter name starts after colon until `/#?`, can access in `ctx.params` object using parameter name as key

```javascript
router.get("/books/:id", async ctx => {
  ctx.body = `Book #${ctx.params.id}.`
});
```

- route parameter name needs to be of RegEx word character class `\w`
- by default route parameters capture everything, can append RegExp in parentheses to restrict, beware: need to escape backslash with another backslash in JavaScript strings ❗️

```javascript
router.get("/books/:id(\\d+)", async ctx => {
  ctx.body = `Book #${ctx.params.id}.`
});
```

- see [path-to-regexp](https://www.npmjs.com/package/path-to-regexp) for full features

### Middleware

- can add middleware function to specific route of router using `router.use(path, middleware)`, e.g. conditional middleware
- is executed for all routes of router that contain the path
- if path is not specified it defaults to `/`, i.e. middleware function is executed for all routes of router

- beware: if there is no route for the endpoint of the request, no middleware is executed, even if the path is contained in that endpoint, i.e. even if path is `/` ❗️

- beware: middleware function is not executed if endpoint of request doesn't match a route of the router, i.e. even if path is `/` ❗️

```javascript
router.use(async (ctx, next) => {
  console.log("Logging request...");
  await next();
});

router.get("/hello", async ctx => {
  ctx.body = "Hello World!";
});
```

```plaintext
<-- GET /hello
Logging request...
--> GET /hello 200 3ms -
<-- GET /world
--> GET /world 404 3ms -
<-- GET /
--> GET / 404 3ms -
```

- beware: middleware function is not executed for routes that don't contain the path, even if endpoint of request is to that path ❗️

```javascript
// never called since doesn't match any route of the router
router.use("/world", async (ctx, next) => {
  console.log("Logging request...");
  await next();
});

router.get("/hello", async ctx => {
  ctx.body = "Hello World!";
});
```

- execution flow is stack-like in order of attachement, stack is combined with handler functions for matching routes, i.e. needs to call `next()` in whoever function is first (middleware or handler) to execute the other one that follows ❗️
- beware: if middleware function is first and doesn't call `next()`, the handler function is never called ❗️

```javascript
router.use(async ctx => {
  console.log("Logging request...");
});

// never called since middleware function doesn't call next(), i.e. /hello is 404
router.get("/hello", async ctx => {
  ctx.body = "Hello World!";
});
```

```plaintext
<-- GET /hello
Logging request...
--> GET /hello 404 3ms -
```

- beware: if handler function is first and doesn't call `next()`, the middleware function is never called ❗️

```javascript
router.get("/hello", async ctx => {
  ctx.body = "Hello World!";
});

// never called since handler function doesn't call next(), i.e. no log
router.use(async ctx => {
  console.log("Logging request...");
});
```

```plaintext
<-- GET /hello
--> GET /hello 200 3ms -
```

- can use second router as middleware function and build nested routers, second router handles only subpath

```javascript
const router = new Router();
const nested = new Router();

router.get("/", async (ctx, next) => {
  ctx.body = "Hello World!";
});

nested.get("/", async (ctx, next) => {
  ctx.body = "Hello nested World!";
});

router.use("/nested", nested.routes(), nested.allowedMethods());

app.use(router.routes());
```

```plaintext
<-- GET /
--> GET / 200 3ms -
<-- GET /nested
--> GET /nested 200 3ms -
```



## Templates

<!-- ToDo: Finish -->

- ???
- use static template files to build HTML
- the template engine replaces variables in a template file with actual values
- e.g. EJS

## Database

<!-- ToDo: Finish -->

<!-- also URL mapping database, uncouple URLs from file names, forward old ones, etc. -->



## Sessions

<!-- ToDo: Finish -->



## Resources

- [Koa](https://koajs.com/), [Koa - Wiki](https://github.com/koajs/koa/wiki)
- [Express - Routing](http://expressjs.com/en/guide/routing.html), [koa-router - API](https://github.com/koajs/router/blob/master/API.md)