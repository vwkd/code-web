---
title: Link
author: vwkd
index: 7
tags:
  - languages
  - html
---

<!-- ToDo: finish -->

???
in `<head>`
rel attribute with type of link
href attribute with URL of link
type attribute with MIME type of resource
  UA will ignore if doesn't support


??? providing the crossorigin attribute to handle the CORS issue



## `media` attribute

- can use `media` attribute to load conditionally based on media query


## `rel` attribute

???

<!-- todo: noopener noreferrer nofollow on user generated links ?! 
on all external links ?!
-->

### `rel="prefetch"`

- preloads resource and caches it
- beware: doesn't execute resource, e.g. styles, scripts, etc. ❗️
- beware: not mandatory unlike `rel="preload"`, UA decides if and when ❗️
- always on lowest priority, only when idle
- specify type via `as` attribute, allows browser to set correct `Accept` header and and use correct CSP
- beware: can also do on HTTP level, using HTTP `Link` header ❗️
- can use for later content, e.g. next web page, etc.
- beware: don't use for resources in markup of same page, UA already does smart fetching based on scroll position, e.g. images ❗️
- beware: fonts need `crossorigin` attribute, otherwise won't be reused due to wrong CORS ⚠️

### `rel="preload"`

- preloads resource and caches it
- beware: doesn't execute resource, e.g. styles, scripts, etc. ❗️
- beware: mandatory unlike `rel="prefetch"`, but UA still decides when ❗️
- semantic, browser can set priority, cache, etc.
- specify type via `as` attribute, allows browser to set correct `Accept` header and and use correct CSP, also allows browser to use right priority
- beware: always set `as` attribute, otherwise will use low priority ❗️
- beware: can also do on HTTP level, using HTTP `Link` header ❗️
- can use for "third-level" content on same page, e.g. dynamic imports in JS, background images and fonts in CSS, etc.
- beware: don't use for resources in markup of same page, UA already does smart fetching based on scroll position, e.g. images ❗️
- beware: fonts need `crossorigin` attribute, otherwise won't be reused due to wrong CORS ⚠️
- beware: for JavaScript modules see `rel="modulepreload"`, otherwise won't be reused due to wrong CORS ⚠️
- can use `onload` attribute to asynchronously load and execute style / script without blocking window's `onload` event

```html
<link rel="preload" as="style" href="style.css" onload="this.rel='stylesheet'">
```

```html
<link rel="preload" as="script" href="script.js"
onload="var script = document.createElement('script');
        script.src = this.href;
        document.body.appendChild(script);">
```

- can use to preload items from JavaScript

```js
const preloadURLs = [
  { href: "a.jpg", as: "image", type: "image/jpeg" },
  { href: "b.png", as: "image", type: "image/png" },
];

const links = [];
preloadURLs.forEach((item) => {
  const link = document.createElement("link");
  link.rel = "preload";
  link.href = item.href;
  link.as = item.as;
  link.type = item.type;
  links.push(link);
});

document.head.append(links);
```

- beware: don't use JavaScript Fetch API to preload items from JavaScript, since not accessible to browser for priority queue, e.g.

```javascript
// bad example
const preloadURLs = ["a.jpg", "b.png"];

window.addEventListener("load", () => {
  preloadURLs.forEach(url => {
      fetch(url)
  });
})
```

### `rel="modulepreload"`

- preloads JavaScript module and caches it
- like `rel="preload"`, except works for JavaScript modules
- also fetches any dependencies linked inside module
- can use to preload JavaScript modules

### `rel="preconnect"`

- establishes connection to origin
- allows later requests to be completed in single roundtrip
- can use if doesn't know resource URL because dependent on runtime values, e.g. image depending on screen size
- beware: if resource URL is known use `rel="preload"` instead ❗️
- beware: can also do on HTTP level, using HTTP `Link` header ❗️



## Resources

- [Demian Renzulli - Prefetch resources to speed up future navigations](https://web.dev/link-prefetch/)
- [Houssein Djirdeh - Preload critical assets to improve loading speed](https://web.dev/preload-critical-assets/)
- [Yoav Weiss - Preload: What Is It Good For?](https://www.smashingmagazine.com/2016/02/preload-what-is-it-good-for/)
- [MDN - Preloading content with rel="preload"](https://developer.mozilla.org/en-US/docs/Web/HTML/Preloading_content)
- [W3C - Preload](https://www.w3.org/TR/preload/)
- [Sérgio Gomes - Preloading modules](https://developers.google.com/web/updates/2017/12/modulepreload)