---
title: Introduction
author: vwkd
index: 1
tags:
  - languages
  - javascript
  - web-apis
---

<!-- todo: mention to read 8. Asynchronicity first -->
<!-- todo: mention some are old CPS API instead of Promises

newer Web APIs use promises, e.g. Fetch API, Battery API, Service Worker API
-->

- usually provided as JS objects with properties and methods, the constructor is called "interface"
- legacy Web APIs used CPS, modern Web APIs use promises
- often constrained by security measures, e.g. permission request, URLs must be HTTPs, etc.



## Examples

- DOM, BOM, CSSOM: interact with page
- Fetch: downloading remote data
- Canvas, WebGL, requestAnimationFrame: draw, animate
- HTMLMediaElement, Web Audio, WebRTC: play media
- Geolocation, Notification, Vibration: device functions
- Web Storage, IndexedDB: client-side storage
- Web Workers: background computation



## AJAX (Asynchronous JavaScript + XML)

- architectural style of web applications that dynamically update content asynchronously
- instead of loading whole new page for every small update, fetches and updates only what changes, e.g. search engine fetches only new results and appends them to page, same for maps, social media, etc.
- name contains XML because data was transmitted in text format XML, today is transmitted in JSON
- makes use of Fetch API, CSS, DOM, etc., name should actually be "Asynchronous JavaScript + CSS + DOM + Fetch API"
- today is standard way of creating web pages, but back in 2005 was totally new approach, therefore got its own name
- AJAX engine: implementation of AJAX, e.g. the function that fetches and updates the changes



## Resources

- MDN - [JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) and individual pages
- Ilya Kantor - [The Modern Javascript Tutorial](https://javascript.info/)
- Douglas Crockford - [Crockford on Javascript - Volume 1-8](https://www.youtube.com/playlist?list=PL7664379246A246CB)
- Jesse James Garrett - [Ajax: A New Approach to Web Applications](https://web.archive.org/web/20150910072359/http://adaptivepath.org/ideas/ajax-new-approach-web-applications/)