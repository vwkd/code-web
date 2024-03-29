---
title: Hypertext Transfer Protocol (HTTP)
author: vwkd
index: 5
tags:
  - the-web
---

- an application layer protocol to transmit files given URL
- designed to transmit hypertext documents on the Web, but can transmit any file, e.g. can replace SSH, FTP, etc.
- communication consists of independent request-response pairs between client, e.g. Web browser, and server
- communication is always initiated by client, server can not respond later asynchronously, i.e. pull-based communication
- stateless, i.e. can't refer to previous requests, e.g. if user is already logged in, where customer is in order process, etc. 👎  
  → need to implement state keeping yourself, e.g. send unique identifier (cookie) in each request to server
- not encrypted, can be intercepted and manipulated, always use HTTPS, see TLS ❗️



## HTTP session

- client sends request to server for a given resource at an URL
- server processes request and sends response back to client
- request is the data of transport layer packet(s)
- request contains file path of resource, doesn't need full URL since the server which reads the request identifies a resource only by its path ❗️
- transport layer packet is sent to port given in URL, by default port 80 for HTTP and port 443 for HTTPS
- internet layer packet is sent to domain name / IP address given in URL
- by default uses one transport layer protocol session per request, i.e. on TCP needs to make new TCP 3-way handshake for every request
- can reuse existing transport layer session for multiple HTTP requests by sending `Connection: keep-alive` header in request, i.e. on TCP no need to make new TCP 3-way handshake for every request
- on single transport layer protocol connection has head-of-line blocking, clients often use multiple connections concurrently using different high-numbered source ports, bad due to increased network load and disabled congestion control, see HTTP/2
- beware: does not change that HTTP itself is stateless, need to still keep track of state, e.g. using cookies ❗️
- beware: there exist only one type of request and response, only the contents change, no other "packets", e.g. from response alone can not necessarily tell which request method was used, see below



## HTTP request

- request for resource at URL
- by convention a server relays a directory path to an HTML file inside it, e.g. `http://example.com/dir/` is interpreted as if it were `http://example.com/dir/index.html` or any other file name
- Web browser sends many requests when loading a single website, often "automatically" in background
  - clicking link or entering URL in URL bar sends `GET` request for resource at URL
  - submitting form sends `POST` (or `GET`) request
  - for each resource referenced in HTML it sends a `GET` request, e.g. images, iframes, external scripts, external styles, etc.
  - for each resource referenced in CSS it sends a `GET` request, e.g. images, fonts, etc.
  - for each resource imported in JavaScript it sends a `GET` request
  - JavaScript can trigger arbitrary requests using Fetch API

### Format

- method: action on resource, e.g. `GET`
- target: path of resource, can also transmit custom data as query after path, e.g. for dynamic website  
  beware: often full URL is used even though not necessary, see [HTTP session](#) ❗️
- headers: metadata about the request, e.g. browser settings, desired content type, desired compression, etc.
- body: data
- for example, a `GET` request to the URL `http://example.com/about/` could look like

```plaintext
GET /about/ HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
```

### Methods

- action on resource on the server
- names are case sensitive
- if safe has no significant side effects on server, except logging, caching, etc.
- if idempotent repeated identical requests yield identical responses

| HTTP method | description | Request body | Response body | Safe | Idempotent | Cacheable |
| - | - |  - | - | - | - | - |
| `GET` | get the resource | Optional | ✅ | ✅ | ✅ | ✅ |
| `HEAD` | get metadata about the resource | Optional | ❌ | ✅ | ✅ | ✅ |
| `POST` | create a subordinate resource | ✅ | ✅ | ❌ | ❌ | ✅ |
| `PUT` | replace / create the resource | ✅ | ✅ | ❌ | ✅ | ❌ |
| `PATCH` | modify the resource | ✅ | ✅ | ❌ | ❌ | ❌ |
| `DELETE` | delete the resource | Optional | ✅ | ❌ | ✅ | ❌ |
| `OPTIONS` | list all methods supported on resource | Optional | ✅ | ✅ | ✅ | ❌ |
| `TRACE` | echo back the request | ❌ | ✅ | ✅ | ✅ | ❌ |

- `GET` is used to fetch a resource from the server without modifying it
- `HEAD` is like `GET`, but only response headers without response body, used to fetch metadata about a resource from the server without transmitting its content, e.g. content-size, content-type, etc.
- `TRACE` is used to see if intermediary servers modified the request
- only `GET and HEAD` must be implemented, most often needs to support only `GET` and `POST`, maybe `HEAD` ??

### Headers
<!-- ToDo: finish -->

- field name and value are separated by colon
- field names are case-insensitive

| request header | description | notes |
| - | - | - |
| Host | domain name and port of server | mandatory with HTTP/1.1 |
| ??? | ??? | |

- `Host` is used to support multiple servers behind one IP, e.g. for each subdomain, because domain name in target was optional, can't enforce easily in backwards compatible way

#### Content negotiation

- accepted representations of resource

| request header | description | notes |
| - | - | - |
| Accept | MIME types accepted as response | |
| Accept-Charset | character sets accepted as response | |
| Accept-Encoding | encodings accepted as response | |
| Accept-Language | languages accepted as response | |
| ??? | ??? | |



## HTTP response

- response for resource at URL
- beware: server may not follow HTTP standards, can do whatever it wants, e.g. may not treat POST/GET as un-/safe, may not honor request headers like `Accept`, may not handle directory paths, may ignore `Host` header, may send wrong status codes, may serve some random file back, etc. pp. ⚠️

### Format

- status code: number representing un-/successful request
- headers: metadata about the response, e.g. timestamp, content type, content character encoding, etc.
- body: file
- for example, a response to a `GET` request to the URL `http://example.com/about/` could look like

```plaintext
HTTP/1.1 200 OK
Date: Fri, 11 Nov 2011 11:11:11 GMT
Server: Apache/2.2.14 (Win32)
Content-Type: text/html; charset=utf-8

<!DOCTYPE html>
<html>
  <p>Hello World!</p>
</html>
```

### Status codes

- status of request

| status codes | description | example |
| - | - | - |
| `1xx` | Information | - |
| `2xx` | Success | `200 OK` |
| `3xx` | Redirection | `301 Moved Permanently` |
| `4xx` | Client Error | `404 Not Found` |
| `5xx` | Server Error | |

### Headers
<!-- ToDo: finish -->

- field name and value are separated by colon
- field names are case-insensitive

| response header | description | example |
| - | - | - |
| Content-Type | MIME type of response | |
| ??? | ??? | |



## HTTP/2

- successor of HTTP 1.1, new versioning scheme
- binary protocol instead of plaintext
- multiplexed streams over single TCP connection, streams can be prioritised, streams can be canceled without canceling TCP connection
- server push, server can send additional data proactively to client, e.g. all resources referenced in a HTML document
- header compression
- most implementations allow use only over TLS
- since uses single TCP connection, again head-of-line blocking, affects all streams, HTTP/1 over multiple TCP connections might even be faster



## HTTP/3

- uses QUIC as transport layer
- streams are outsourced to underlying QUIC, priorization is still done in application layer
- not over TLS, directly over QUIC, since TLS is built into QUIC
- otherwise like HTTP/2



## Resources

- [Wikipedia - HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) and subpages
- [MDN - HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) and subpages
- [http2 explained](https://daniel.haxx.se/http2/), [HTTP/2 - FAQ](https://http2.github.io/faq/), [HPBN - HTTP/2](https://hpbn.co/http2/)
- [http3 explained](https://daniel.haxx.se/http3-explained/)