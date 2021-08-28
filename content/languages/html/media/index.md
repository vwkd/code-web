---
title: Media
author: vwkd
index: 6
tags:
  - html
---
# Media



## Single source: `<img>`

- single source
- empty element
- `alt` attribute: alternate text
- beware: don't omit `alt` attribute, instead use empty string if image isn't content relevant, won't be announced by screen reader and UA won't show failed to load indicator incase of error 
- `width` / `height` attribute: width / height in pixels, integer without a unit, overwritten by CSS
- beware: always specify `width` / `height` attributes to reserves space in layout until it loads ❗️
- beware: if only one CSS property is specified the other falls back to the attribute value, needs to overwrite it using `auto` value, e.g. `p { width: 100%; height: auto; }` ❗️

### Single size

- `src` attribute: source URL

```html
<img src="logo.jpg" alt="Company logo">
```

### Multiple sizes

- `srcset` attribute: list of source URLs and corresponding sizes, e.g. `srcset="logo_s.jpg 1x, logo_m.jpg 2x"`
- `sizes` attribute: list of media query and desired size, UA chooses best match from `srcset`, no effect if `srcset` is missing, also sizes in `srcset` must be width instead of pixel ratio
- use `src` as fallback for UAs that don't support `srcset` and `sizes`

```html
<img srcset="logo_s.jpg 200w, logo_m.jpg 500w, logo_l.jpg 1000w" sizes="(max-width: 600px) 200px, 50vw" alt="Company logo" src="logo_s.jpg">
```

- beware: `srcset` attribute is for different sizes of same image, for different images (including formats) use `<picture>` with `<source>` elements instead ❗️



## `<source>`

- a media resource for `<picture>`, `<audio>` and `<video>`
- can use multiple sources for different resources, e.g. square or landscape, `.webp` or `.jpg`, etc.
- UA chooses first source whose `type` it supports and whose `media` matches
- beware: UA ignores any following source, even if it would support it ❗️
- empty element
- `src` attribute (only within `<audio>` / `<video>`): URL of resource
- `type` attribute: MIME type of resource
- `media` attribute (only within `<picture>`): media query, e.g. `media="(min-width: 600px)"`
- `srcset` attribute (only within `<picture>`): list of source URLs and corresponding sizes, e.g. `srcset="logo_s.jpg 1x, logo_m.jpg 2x"`
- `sizes` attribute (only within `<picture>`): list of media query and desired size, UA chooses best match from `srcset`, no effect if `srcset` is missing, also sizes in `srcset` must be width instead of pixel ratio



## Multiple sources: `<picture>`

- multiple sources
- not empty element
- content is multiple `<source>` elements
- fallback is `<img>` element
- beware: always specify width and height as CSS inline styles to reserves space in layout until it loads ❗️
- beware: for different sizes of same image use `<img>` with `srcset` attribute instead of `<picture>` with `<source>` elements ❗️

### Single size

- single value in `srcset` of each `<source>`

```html
<picture>
  <source srcset="square.jpg" type="image/jpeg" media="(max-width: 650px)">
  <source srcset="rect.jpg" type="image/jpeg" media="(min-width: 650px)">
  <img src="circle.jpg" alt="Amazing landscape">
</picture>
```

### Multiple sizes

- multiple values in `srcset` of each `<source>`

```html
<picture>
  <source srcset="square_s.jpg 200w, square_m.jpg 400w" type="image/jpeg" media="(max-width: 650px)">
  <source srcset="rect_l.jpg 800w, rect_xl.jpg 1000w" type="image/jpeg" media="(min-width: 650px)">
  <img src="circle.jpg" alt="Amazing landscape">
</picture>
```



## `<video>`

- not empty element
- attributes: `controls`, `autoplay`, `loop`, `muted`, etc.
- beware: always specify width and height as CSS inline styles to reserves space in layout until it loads ❗️
- formats (supported by HTML5): MP4, WebM, Ogg, etc.

### Single source

- uses `src` and `type` attributes
- fallback is content

```html
<video src="video.webm" type="video/webm" controls muted>Video not supported.</video>
```

### Multiple sources

- uses `<source>` elements
- fallback is content

```html
<video controls muted>
    <source src="video.webm" type="video/webm">
    <source src="video.mp4" type="video/mp4">
    <p>Video not supported.</p>
</video>
```



## `<audio>`

- not empty element
- attributes: `controls`, `autoplay`, `loop`, `muted`, etc.
- beware: always specify width and height as CSS inline styles to reserves space in layout until it loads ❗️
- formats (supported by HTML5): MP3, WAV, Ogg (and MP4 from video), etc.

### Single source

- uses `src` and `type` attributes
- fallback is content

```html
<audio src="audio.mp3" type="audio/mpeg" controls>Audio not supported.</audio>
```

### Multiple sources

- uses `<source>` elements
- fallback is content

```html
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <p>Audio not supported.</p>
</audio>
```



## `<iframe>`

- embed other web documents into current document, e.g. YouTube video, Google Maps map, Disqus comments, ad banners
- not empty element
- attributes: src, frameborder, allowfullscreen, sandbox etc.
- specify width and height via CSS, otherwise browser can’t reserve space while loading the page and it could flicker
- fallback: content
- performance: use JS to set src after page is loaded to improve loading speed
- security: be cautious what you embed, only use HTTPS because over HTTP embedded content can access contents of own document, use sandbox attribute to stop script execution, popups, form submission etc., configure own server to deny own pages being framed by configuring it to send X-Frame-Options header



## `<object>`

- general purpose embedding, e.g. plugins, web pages, pdfs, images
- rarely needed today, were used in old days, today use HTML5 media elements, iframes, link to pdfs, abandon plugins etc.
- attributes: data, type
- parameters for plugin: attributes in one \<param\> element in content
- specify width and height via CSS, otherwise browser can’t reserve space while loading the page and it could flicker
- fallback: content



## `<embed>`

- like `<object>` element, but less often used
- empty element
- attributes: src, type
- parameters for plugin: attributes in element itself
- specify width and height via CSS, otherwise browser can’t reserve space while loading the page and it could flicker
- fallback: no



## Resources