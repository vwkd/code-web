---
title: Viewport
author: vwkd
index: 3
tags:
  - languages
  - css
---

- visible area where document is rendered, i.e. user agent window excluding menu bar, etc.
- not affected by overlay controls, e.g. scrollbars, on-screen keyboard, etc.
- doesn't include overflow area
- some replaced elements have viewport for contents, isolated from top-level viewport, content doesn't "know" of outside world, e.g. iframe, object, SVG, etc.
- 2D coordinate system, x-axis to right, y-axis to bottom, origin in top left corner
- many properties are (eventually) relative to viewport, e.g. default width of root element, etc.



## Zoom

- magnification, enlargement of size
- scales length units

### Text zoom

- default font-size in user agent settings
- scales initial value of `font-size` property, by default `medium` (≙ `16px`)
- therefore scales font-relative units that (eventually) depend on initial value of `font-size`, e.g. `font-size` of root element, outside of property values, etc.
- doesn't scale other length units, e.g. font-relative units that don't (eventually) depend on initial value of `font-size`, line-relative units, viewport-relative units, absolute units, etc.
- can get overflow due to not scaling viewport
- usually affects layout of page
- can enable / disable by using only units that scale / don't scale

### Page zoom

- zoom in menu, keyboard zoom
- scales pixel unit, i.e. not closest integer to reference pixel anymore but some multiple
- therefore scales all other length units with it, see Data Types#Relative_Units
- size of viewport stays the same from perspective of user, its size shrinks from perspective of page, its size in pixels shrinks
- all units that (eventually) depend on viewport size stay same from perspective of user, shrink from perspective of page, e.g. viewport-relative units, percentages, other relative units, etc.
- `position: fixed` elements don't move since relative to viewport, disrupt layout
- can get overflow due to not scaling viewport
- usually affects layout of page
- replaced by page scale
- can enable / disable by using only units that scale / don't scale

### Page scale

- zooming gestures, e.g. pinch-to-zoom
- scales pixel unit and viewport, see Layout and Visual Viewport
- `position: fixed` elements move, don't disrupt layout anymore
- can't get overflow due to scaling of viewport
- doesn't affect layout of page
- replaces page zoom
- can't enable / disable since always enabled, page doesn't "know" it's scaled, no work necessary 🎉
- beware: affects only VV of top-level window, not VV of replaced elements, e.g. iframe, object, SVG, etc. ❗️



## Layout and Visual Viewport

- viewport from perspective of page and user, separate notions for same thing
- beware: definition of viewport from perspective of user, from perspective of page on imaginary screen that scales with it, as if the user scaled up with it, page doesn't "know" (\*) that only part of it is visible on real screen ❗️
- beware: assumes screen media, not paged media ❗️
- logical size: size from perspective of user, in physical units (real, not CSS)
- virtual size: size from perspective of page, in pixel unit
- pixel unit is from perspective of page, shows virtual size not logical size, e.g. logical size can stay the same even though size in pixel shrinks
- layout viewport (LV): viewport from perspective of page, doesn't change virtual size if pixel unit is scaled
- visual viewport (VV): viewport from perspective of user, doesn't change logical size if pixel unit is scaled
- VV is affected / LV is not affected by overlay controls, e.g. scrollbar, on-screen keyboard, etc.
- if pixel unit is scaled up (positive zoom), virtual size of LV stays same and VV shrinks, logical size of LV grows and VV stays same
- in both logical and virtual size VV is smaller than LV
- if pixel unit is scaled down (negative zoom), virtual size of LV stays same and VV grows, logical size of LV shrinks and VV stays same
- in both logical and virtual size VV is bigger than LV
- if pixel unit is not scaled (no zoom), both logical and virtual size of LV and VV are the same
- layout of page doesn't change since LV and pixel unit scale together, page doesn't "know" (\*) it's scaled, like time dilated system that doesn't notice it's time dilated, as if user just had a magnifying glass
- values in page that depend on viewport are relative to LV, fixes problems of page zoom, e.g. viewport-relative units, `position: fixed` elements, CSSOM properties like `windows.innerWidth`/`innerHeight`, etc.
- (\*) see Visual Viewport API how page can read VV, e.g. fix elements to VV instead of LV, etc.



## Visual Viewport API

- API to read VV from within page
- in `visualViewport` global object
- read-only properties
- can use to fix position of elements to VV instead of LV, e.g. image controls, text formatting bar, etc.
- events `resize` and `scroll` on `visualViewport` when VV changes, don't bubble
- beware: replaced elements with viewport have own `visualViewport`, useless since VV is same as LV, e.g. iframe, object, SVG, etc. ❗️



## Initial size

- user agent can set initial size of viewport different from device width, emulate different screen size
- scales the pixel unit without scaling the (layout) viewport, i.e. identical to page zoom
- beware: not zoom, has nothing to do with LV and VV ❗️
- user agent on mobile phones scale pixel unit down, like negative page zoom, emulate larger screen to layout unoptimised websites better, can then use page scale to zoom in

### Viewport meta tag

- tag in HTML head to set initial size of viewport
- not official, browser inconsistencies
- only supported by user agents on mobile devices
- use to set viewport to device width, i.e. disable emulation of different screen size

```html
<meta name="viewport" content="width=device-width">
```

- `width`: width of viewport in pixel unit, pre-defined constant `device-width`, defaults to emulated screen size, e.g. `980px` on iOS Safari
- `height`: height of viewport in pixel unit, pre-defined constant `device-height`, defaults to fit aspect ratio of device to `width`
- specifies pixel unit through desired value for size of viewport, inverse logic
- beware: avoid zoom-related properties, inconsistent behavior, e.g. `initial-scale` to set value of initial zoom ❗️
- beware: don't use if page is not optimised for all screen sizes, otherwise stuck in same problem that user agent tried to solve for unoptimised websites ❗️
- beware: should have been CSS instead of HTML since presentation not semantics, but needs to be known before HTML body is parsed, e.g. for responsive images, `@viewport` was removed from CSS spec since not implemented by browsers, see [#258](https://github.com/w3c/csswg-drafts/issues/258), [#4766](https://github.com/w3c/csswg-drafts/issues/4766) ❗️



## Resources

(beware: resources show browser inconsistencies, bugs mostly fixed by now, e.g. `position: fixed` elements relative to VV instead of LV, CSSOM properties relative to VV instead of LV, etc. ❗️)

- [Peter-Paul Koch - Viewports visualisation app](https://www.quirksmode.org/mobile/viewports/)
- [Peter-Paul Koch - The Mobile Viewports](https://www.youtube.com/watch?v=8J6EdpXdzqc)
- [David Bokan - Visual Viewport Demo](https://bokand.github.io/viewport/)
- [David Bokan - Web Viewports Explainer](https://github.com/bokand/bokand.github.io/blob/master/web_viewports_explainer.md)
- [webhint - Meta Viewport](https://webhint.io/docs/user-guide/hints/hint-meta-viewport/)