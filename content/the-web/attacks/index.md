---
title: Attacks
author: vwkd
index: 15
tags:
  - the-web
---

<!-- todo: finish reading

https://en.wikipedia.org/wiki/Cross-site_request_forgery#Cookie-to-header_token
https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#encryption-based-token-pattern
https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
 -->

- techniques to exploit vulnerabilities in websites



## Cross Site Scripting (XSS)

- attack that forces user of a website to execute unwanted JS, e.g. read cookies
- relies on JS having full access to all resources on page
- exploits trust that user has for domain
- beware: can make CSRF through XSS, or XSS through CSRF ❗️
- reflected XSS: JS is in HTTP request and reflected back in HTTP response, e.g. in parameter, search query, invalid path of URL, etc.
- stored XSS: JS is stored on website, e.g. in database, third party library, browser extension, etc.
- exploit: inject JS into page, e.g. web page that accepts user input without sanitizing it



## Cross-Site Request Forgery (CSRF)

- attack that forces user of a website to execute unwanted request, e.g. delete resource
- relies on cookies being sent with every request
- exploits trust that domain has for user
- beware: can make CSRF through XSS, or XSS through CSRF ❗️
- exploit: inject URL with sideffect, e.g. link, image, hidden form, etc.




# CSRF protection

<!-- TODO: FINISH -->

for same-origin requests
idea: send secret in response, client sends in next request
    since requires previous response, attacker doesn't know
i.e. requires previous response, requires initial page load before state-changing request
only use for state-changing request, e.g. password update, etc.

client doesn't store secret, just reuses it
server can rotate it with every request, but breaks back button on client, must not cache HTML document
server can rotate it with session itself, less secure since attacker might be able to steal it with XSS and reuse directly

## CSRF token

can be stateful or stateless (on server)
either random value stored in session
or user ID plus expiry time signed

??? like one-time-use session token for next request
    (client doesn't know that server might reuse it)

s-c token needs only userID and expiry time
    no scopes or any other info
    just for verifying that request is same-origin

## CSRF token flow

### hidden form field

only good for forms
no JS necessary, client doesn't need to know

### custom header

good for any kind of request
requires JS to build request

### custom body

good for any kind of request
requires JS to build request



## Implementation

log failed tokens

//////////// old

<!-- todo: consider moving to 15. -->

- temporary secret
issued with response, 

that's issued to client shortly before
?? not as cookie but such that isn't send by a clever construction of a link

- server generates token, stores in session, sends to client
- client sends back in subsequent request(s)
- server verifies that token matches
- verifies that request is same-origin and not cross-origin
beware: only necessary if cookie is not `SameSite: Strict` already

- token must be same as session token, e.g. unique, random, etc.

### Lifetime

- single request
    adv: smaller attack window to steal
    disadv: can't use same session on multiple windows, e.g. multiple tabs
        ?? also back button won't work since cached invalid token
    -> must not cache HTML document
- same as session
    opposite of above

### ?? Transmission

#### Stateful


#### Stateless

- in hidden form field, will become request parameter
- in custom header, JS reads out, construct own header
    only works for JS requests

- random, unique value

essentially limits requests to same-origin requests
    ?? could just as well use SameSite on cookie?

- beware: name should have been "anti CSRF token" ❗️

- can use for any state-changing requests, e.g. login, register, send money, etc.

?? what happens on multiple tabs open ??



should log
should revoke user session



<!-- oldoldold -->
random token set in cookie `X-CSRF-TOKEN`, non-HttpOnly
JS reads cookie and sends token back in custom request header (or in hidden form field)
since only JS on own page can read cookie
-> in SPA (written in JS) can use CSRF-cookie (anti-forgery token, CSRF token) as protection, random token set in cookie `X-CSRF-TOKEN` non-HttpOnly, JS reads cookie and sends token back in custom request header (or in hidden form field), since only JS on own page can read cookie
  but needs JS to make requests
  when requesting /login (or /register), server sets csrf token as cookie and in hidden input field, in request (POST /login (or /request) route) compares csrf token in cookie and form field
  match only if on own page, because only own page can set cookie ?!
  beware: use CSRF token for any mutating action





## Resources

- [OWASP - Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
- [OWASP - Cross Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf)