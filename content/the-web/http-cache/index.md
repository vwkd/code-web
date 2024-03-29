---
title: HTTP cache
author: vwkd
index: 12
tags:
  - the-web
---

- proxy between client and server that serves client stored responses from similar previous requests
- forwards request it hasn't already seen, stores and forwards response
- intercepts request that it has seen already, serves stored response
- stores response only temporarily
- like assistant that takes load off of boss by doing repeated tasks
- advantages:
  - lower latency, because request travels shorter distance
  - lower bandwith, because network behind proxy doesn't see traffic
  - less load, because server doesn't see traffic
- disadvantages:
  - complexity, because additional node in network
  - stale content, because server can't tell proxy to update content (except a public proxy it controls)
  - security risk, because proxy reads every request and response
- beware: often used as synonym for browser cache ❗️
- beware: never rely on cache, neither on server nor on client, resources may not be cached, e.g. limited disk space, deletion by user, expiration, etc. ❗️



## Cache types

- can be private or public

### Public / Shared

- on network, e.g. ISP, CDN, load balancer, etc.
- needs to termiante HTTPS
- serves multiple users, "shared"
- used often in front of dynamic Web server

### Private

- on client, e.g. memory cache, browser cache, Service Worker, HTTP/2 push cache, etc.
- doesn't need to terminate HTTPS, because not yet on network
- serves single user
- used in every client



## Cache settings

- server specifies in response headers
- beware: don't use `Cache-Control` request header, doesn't make sense ⚠️
- `Cache-Control: #<directive>`: deactivation, location, expiration, validation, multiple directives are comma-separated
- `Etag: <token>`: validation token
- beware: client shouldn't specify cache settings, because doesn't know what resource is, server knows best because by definition its the resource owner ⚠️
- beware: client shouldn't send `Cache-Control` request headers to cache to change cache behavior, cache shouldn't send `Age` response header to client to show age of cache entry ⚠️
- beware: always set cache explicitly, otherwise cache can do whatever it wants, e.g. browser by default caches GET but not POST ❗️
- beware: browsers never cache PUT or DELETE no matter if cache is set explicitly or not ❗️

### Deactivation

- if response is cached
- `Cache-Control: no-store`: don't cache
- beware: confusing name, should have been `no-cache` ❗️
- `Cache-Control: no-cache`: like `no-store` but more efficient, shorthand for `max-age=0, must-revalidate`, fastest way to get always fresh response
- beware: makes sense only with validation, otherwise is less efficient than `no-store` because cache is wasted space since never used ❗️
- beware: confusing name, should have been `must-revalidate` ❗️
- if response always changes between two requests use `no-store`, if response doesn't always change between two requests use `no-cache`
- if omitted and any other `Cache-Control` is set then caches

### Location

- where response is cached
- `Cache-Control: private`: in private cache only
- `Cache-Control: public`: in any cache, both public and private
- if omitted uses "best guess", usually public cache doesn't cache, i.e. as if defaults to `private`

### Expiration

- how long response is cached, time until cache entry expires
- `Cache-Control: max-age=<seconds>`: relative to time of request
- `Cache-Control: s-maxage=<seconds>`: like `max-age` but only respected by public caches
- can specify both, public cache prefers `s-maxage`
- beware: replaces older `Expires` header with absolute time ❗️
- `Cache-Control: must-revalidate`: disallow using expired cache entry, expired cache entry is used only in exceptional circumstances, e.g. when server is down
- beware: don't use usually, only relevant if existence of response is confirmation in itself, e.g. financial transaction ❗️
- beware: confusing name, should have been `no-expired` ❗️
- if omitted uses "best guess"
- beware: some browsers (Firefox, Safari as of Jan 2021) ignore expiration when user manually reloads page, always request page from server even if has cache entry (conditionally if has cache entry with validation token), needs to set `Cache-Control: immutable` to use cache entry instead as expected ❗️

### Validation

- instead of throwing away cache entry after expired, can keep it and validate with server on next client request if can continue to use it
- cache asks server if updated, if not just reuses cache entry, if did fetches new cache entry
- server generates token based on resource, usually fingerprint of content, e.g. hash
- server sends token in `ETag` response header, opaque to cache
- after cache entry expired, cache sends conditional request, sends token in `If-None-Match` request header
- if fingerprint didn't change, server sends headers only with `304 Not Modified`, cache reuses expired cache entry until new expiration from headers
- if fingerprint changed, server sends resource with `200 OK`, cache updates cache entry
- beware: replaces older `Last-Modified` response header and `If-Modified-Since` request header that used timestamp, weaker validator because constrained by resolution of seconds ❗️



## Cache entry

- representation of resource
- stored as key value pair
- key is URL and method if caches multiple methods
- value is response
- beware: a cache entry may be shared across websites just the same way the underlying resource may be shared across webpages ❗️

### Variation

- resource might have multiple representation that vary based on content negotiation, e.g. `Accept-Encoding`, `Accept-Language`, etc.
- cache breaks website on client if doesn't distinguish multiple representations of same resource, e.g. wrong encoding, wrong language, etc.
- server specifies request headers for content negotiation in `Vary` response header, e.g. `Vary: Accept-Language`
- cache uses varied request headers as additional keys
- beware: cache must normalize request header values since can differ between clients, otherwise creates duplicate cache entries, e.g. `Accept-Language` could be `fr-FR` or `fr_FR`, etc. ⚠️
- usually only relevant for public cache, since for same client content negotiation usually doesn't change



<!-- ToDo: finish -->

## Updating resources

- problem: update resource which is cached, when server can't update cache (except a public proxy it controls)

- if a resource is updated on the server, then in a cache can have exist either one of two versions, since a cache can have either still the old or already the new version
- the server loses control over its own resource on the client, needs to wait until cache entry expired to be sure
- for a single resource this is not too bad, is just outdated
- but resources are almost always interdependent with other resources, e.g. assets on a webpage, webpages on a website, etc.
- assume that any resource is interdependent, because almost all are except maybe a direct download link
- mixing new and old versions of interdependent resources can create breakages because the update can create an incompatibility between the old version of one resource and the new version of another resource
- updating two interdependent resources is enough to create a breakage if old version of one and new of other aren't compatible
- 

- must never mix old and new versions, because can't guarantee that old and new versions are always compatible
- but can't guarantee this, because can't rely on existence of any cache entry, no matter the cache expiration, e.g. might have expired, might have been deleted, might not exist yet, etc.
- cache has fail-open behavior that loads new version, assume resource can be new version at any point in time
- assume if there are old and new versions of a resource, then eventually gets a mix of them

- must not create old version, because then it's always the new version
- either don't cache or don't update a resource
- beware: it's not enough to minimize old versions in cache, must make sure there are never old versions in the cache in the first place ❗️

### Revving

- change resource URL when content changes, allows to not update a resource
- creates new resource for each update, doesn't update existing resource ever
- set long cache duration, e.g. 1 year
- caches resource until it updates, almost perfect cache, except that old version is left lying around wasting storage
- can use fingerprint, version number, random string, etc.
- prefer fingerprint based on contents, doesn't change between versions if content doesn't change, cache is not busted if didn't change, e.g. hash
- use build workflow, e.g. modify filename, etc.
- can only rev "internal" URLs, because can change without breaking things, e.g. assets, etc.
- beware: don't rev external URLs, because can not change, e.g. HTML documents, links from other websites, etc. ⚠️



## Cache strategy

- don't cache responses for requests that change with query parameters or request body, e.g. GraphQL API, etc.
- for "internal" / non-user-facing resources use revving and long cache, e.g. assets, etc.
- for "external" / user-facing resources don't cache, e.g. HTML documents, HTTP API, etc.
- beware: if does cache "external" resource risks breakages due to incompatibilities from mixing of new and old versions of interdependent resources ⚠️
- cache personalised responses at most in private cache, e.g. profile page, etc.
- beware: never cache personalised responses in public cache, otherwise second client can see response for first ❗️
- usually caches only responses with certain status codes, e.g. &lt; 500 because server error is not normal
- beware: caching and not caching a resource doesn't correspond to a static / dynamic Web server, e.g. can statically serve a changing resource, can dynamically serve a non-changing resource ❗️



## Resources

- [web.dev - Prevent unnecessary network requests with the HTTP Cache](https://web.dev/http-cache/)
- [Jake Archibald - Caching best practices & max-age gotchas](https://jakearchibald.com/2016/caching-best-practices/)
- [Mark Nottingham - Caching Tutorial](https://www.mnot.net/cache_docs/)
- [Yoav Weiss - A Tale of Four Caches](https://blog.yoav.ws/tale-of-four-caches/)