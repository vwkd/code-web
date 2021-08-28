---
title: URL (Uniform Resource Locator)
author: vwkd
index: 4
tags:
  - the-web
---

- unique address of a resource on the Internet, like IP address for single piece of information, e.g. document, song, video, etc.
- often using domain names to make links more human-readable, although could as well use IP address
- beware: not necessarily resource only on the Web, e.g. `ftp://example.com/file.txt` ❗️
- beware: resource is entirely up to server, may correspond to actual file at given path, or server may generate it dynamically, or reply with something else entirely, e.g. `http://example.com/file.txt` can be anything, doesn't need to be a text file, could be anything ❗️
- beware: should never change, breaks all existing links to resource, at least set up server to forward to new URL ❗️
- beware: choose name that doesn't change, leave out any temporary information, e.g. no file extension, etc. ❗️



## Syntax

- `scheme://prefix.domain:port/path/filename`, e.g. `http://example.com/path/to/some/file.html`
- can contain only certain safe characters: `A-Za-z0-9-_.~`
- other characters need URL encoding: `%` + 2 digit hex representation of UTF-8 code point, e.g. `ö` is `%C3%B6`, space is `%20`, `+` is `%2B`
- better syntax would have been structured like DNS tree, e.g. `http:com/example/foo/bar/baz`
- beware: URL is not only file path of resource, it contains scheme, domain, port, as well as query and fragment ❗️
- beware: path may be treated case in-/sensitively depending on server, always use lowercase to avoid ambiguity ❗️

### Query

- send parameters and values with request, e.g. `https://en.wikipedia.org/w/index.php?search=Internet+provider`
- server can use parameters to customise request, e.g. search results
- can append query after `?` to URL
- space in a value is usually represented by `+`
- multiple parameters are usually separated by `&` or `;`

### Fragment

- points to a part of the resource, e.g. `https://en.wikipedia.org/wiki/Internet#History`
- is evaluated only by client, not send to server
- can append a fragment after `#` to URL



## Relative URL

- URL specified by file path relative to existing (absolute) URL, e.g. in hyperlink in document
- uses UNIX file path syntax, e.g. given the (absolute) URL `https://example.com/foo/bar/`

  | relative URL | resulting URL |
  | - | - |
  | `/cat/` | `https://example.com/cat/` |
  | `cat/` | `https://example.com/foo/bar/cat/` |
  | `../cat/` | `https://example.com/foo/cat/` |



## Hyperlink

- link to a resource that can load by clicking
- most often the resource is on the internet, i.e. has a URL
- in a HTML document is an anchor element with the URL of the resource
- often used as synonym for URL, e.g. "sending a link", but URL is the address while link is the object that can be clicked ❗️
- can use relative URLs, taken relative to current document URL
- relative URLs are domain independent, automatically use HTTPS 🎉
- historically is styled blue with underline
- using fragment in URL can link to any element on a page with unique `id` or `name` attribute, e.g. anchor on same page with `href="#myid"` jumps to element with id `myid`



## Resources

- [Wikipedia - URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)
- [Tim Berners-Lee - Why the //, #, etc?](https://www.w3.org/People/Berners-Lee/FAQ.html#etc)
- [Wikipedia - Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding)