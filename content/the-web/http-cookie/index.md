---
title: HTTP cookie
author: vwkd
index: 11
tags:
  - the-web
---

- key value pair stored by UA for a website
- UA sends cookie for every subsequent request to same website
- sent for specified URLs (on same domain), see `Domain` attribute and `Path` attribute
- stored for specified duration, see `Expires` attribute and `Max-Age` attribute
- used to maintain session across multiple request, makes HTTP stateful, e.g. authentication, tracking, etc.
- beware: don't confuse with cache, cache stores resources (with URL) and is not sent back to server ❗️
- beware: store only data that server needs to know with every request, e.g. authentication, otherwise use local storage like Web Storage or IndexedDB, e.g. preferences (theme, language, banner dismissed) ⚠️
- beware: never rely on existence, may stop to exist at any point in time, e.g. space constraint, switching browser, user deletes it, etc. ❗️
- beware: never trust since can be manipulated like any other user input, always sanitise content and validate, e.g. malicious UA could send expired cookie, etc. ❗️
- beware: never store sensitive data of user or server since can be extracted, instead store on server ❗️
- beware: store as little data as possible since increases size of requests ❗️
- beware: corresponds to single browser instance not single user, since single browser may be used by multiple users or single user may use multiple browsers ❗️
- beware: sent for any kind of requests, e.g. user navigation, linked resource, `fetch` in JS, etc. ❗️
- beware: origin domain of request does not need to be domain for which cookie is set, e.g. page with top-level navigation to external resource (e.g. link, popover, `<link rel="prefetch">`, etc.), page that loads external resource (e.g. image, iframe, etc.), page that fetches external resource through JS, etc. ❗️



## Setting

- can set via `Set-Cookie` response header(s)
- `Set-Cookie: <name>=<value>; <attributes>`
- name and value limited to certain characters, e.g. no white space, comma, semicolon, equal sign, etc.
- can also set via JS `document.cookie` API
- sent by UA in `Cookie` header, only name value pairs without attributes, multiple cookies separated by semicolon



## Attributes

- comma separated in header
- beware: invalid attribute are silently ignored ❗️

### Scope

- URLs for which cookie is sent by UA
- "current X" refers to X in URL of resource that set the cookie, e.g. current domain, current path, etc.

#### `Domain`

- domain for which cookie is sent
- can be current domain or super domain
- must include current domain, e.g. `auth.foo.com` can set for `foo.com` but `foo.com` not for `auth.foo.com`
- must not be another domain, e.g. `foo.com` can't set cookie for `bar.com`
- if not specified, defaults to current domain without sub domains
- if specified, includes any sub domains
- beware: if specified, is less restrictive than default ❗️

#### `Path`

- path for which cookie is sent
- can be any path
- includes any sub paths, e.g. `foo.com/` includes `foo.com/blog/`
- beware: doesn't need to include current path, e.g. `foo.com/blog/` can set for `foo.com/` ❗️
- if not specified, defaults to current path

### Duration

- time for which cookie is stored by UA
- defaults to end of session
- beware: session doesn't necessarily mean when UA is closed, e.g. restore session, etc. ❗️
- calls "session cookie" if default duration, otherwise “persistent cookie”
- beware: persistent cookie might live shorter than session cookie if duration is shorter than duration of session ❗️
- can only set either `Expires` or `Max-Age`, if both then `Max-Age` takes precedence

#### `Expires`

- HTTP-date timestamp
- if in past, expires immediately

#### `Max-Age`

- number of seconds from when resource was received
- if non-positive, expires immediately

### Security

- mitigate attacks, e.g. XSS, man-in-the-middle, CSRF, etc.
- beware: doesn't make cookie itself secure, data can still be manipulated by user ⚠️

#### `Secure`

- cookie is sent only for HTTPS requests
- boolean attribute
- beware: should always use ❗️
- beware: should have been default, and `Insecure` be option ❗️
- mitigates man-in-the-middle attack

#### `HttpOnly`

- cookie can not be accessed through JS
- boolean attribute
- beware: should always use ❗️
- beware: should have been default, and `JsAlso` be option ❗️
- beware: still sent for requests originating from JS ❗️
- mitigates XSS attack

#### `SameSite`

- cookie is sent only for requests from certain origins (domains)
- can set to `Strict`, `Lax`, or `None`
- defaults to `Lax`

##### `Strict`

- same-origin requests, i.e. no state when coming from Google, needs to reload page, etc.
- mitigates CSRF

##### `Lax`

- like `Strict`, but also for cross-origin request through top-level navigation
- mitigates CSRF only partially, since can still make popover, `<link rel="prefetch">`, etc.

##### `None`

- cross-origin requests, i.e. like previously
- doesn't mitigate CSRF



## Third-party cookie

- first-party cookie: cookie set by own resource (of same domain as domain of website)
- third-party cookie: cookie set by foreign resource (of other domain than domain of website), e.g. external ads, external images, etc.
- beware: first-/third-party cookie is relative to what website is currently visiting ❗️
- third party recieves its cookie on every request for its resources from any page (given it uses `SameSite=None`)
- third party can use to keep session accross websites, e.g. YouTube in embedded iframe
- third party can use to track user accross websites
- beware: don't confuse with scope, each party can still only see their own cookies
- UAs will block third party cookies by default, use Storage Access API instead



## Regulations

- cookies are regulated in many countries, e.g. limited duration, etc.
- needs user's constent except when "strictly necessary"



## Resources

- [Rowan Merewood - SameSite cookies explained](https://web.dev/samesite-cookies-explained/)
- [gdpr.eu](https://gdpr.eu/cookies/)