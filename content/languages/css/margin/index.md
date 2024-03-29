---
title: Margin
author: vwkd
index: 6.1
tags:
  - languages
  - css
---

- transparent area of box outside of border
- spaces box from other boxes in Flow Layout
- beware: affects layout, not box itself, same as gutters in other regular layouts, but made part of box ⚠️
- beware: should not have been part of box, should have used gutters in Flow Layout ❗️
- beware: use only in Flow Layout, use gutters in other regular layouts ❗️
- beware: still has effects in other regular layouts, e.g. Flex Layout, Grid Layout, etc. ❗️
- beware: don't use for alignment, instead use alignment properties, see Alignment ❗️
- beware: here use logical directions instead of physical, see Writing Mode ❗️
- beware: historical bug of margin collapse, see Margin Collapse ⚠️



## Margin properties

- specify thickness of margin area
- longhand `margin-*` for each side, shorthand `margin` for all four sides
- not inherited
- beware: block direction margins have no effect on inline-level boxes, see Flow Layout#Inline_Layout ❗️
- beware: use margins only in one direction, space has single source instead of two, avoids margin collapse (except parent with child), e.g. bottom and left and not top and right ❗️



## Margin values

- values: `<length-percentage>`, `auto`
- initial value: `0`, not `auto` ❗️
- percentage relative to _inline size_ of containing block, i.e. _width_ in horizontal writing mode, see Writing Mode
- beware: even for `margin-block-start` and `margin-block-end`, allows same value to make equal sized margin in any direction ❗️
- can be negative, creates "negative space", i.e. shifts box in opposite direction
- beware: negative margin can create overlap, for stacking order see Stacking, e.g. ODT `inline` elements are above ODT `block` elements ❗️
- beware: negative margins can create overflow, see Overflow ❗️
- beware: user agent style sheet may set any value, overrides initial value, e.g. `body { margin: 8px; }` ❗️

### `auto`

- computes to zero
- except if both inline direction margins on block-level elements are `auto`, and its inline size is smaller than the available space, they each compute to half of available space, i.e. box is centered in inline direction
- beware: inline size isn't necessarily non-`auto`, just for elements for which `auto` inline size computes to `100%`, e.g. `auto` for replaced element is intrinsic size, e.g. `<img>` ❗️
- beware: centering behavior also applies if relatively / stickily positioned, but not if absolutely / fixedly positioned ❗️
- beware: centering behavior doesn't apply to block direction margins, instead use Flex Layout ❗️

```html
<div class="center"></div>
```

```css
body {
  /* to make size 100vw/vh not overflow */
  margin: 0;
  /* to make border not overflow */
  box-sizing: border-box;
  /* base for size percentages in children */
  /* width is already 100% by default */
  height: 100vh;
  /* width: 100vw; */
  border: 1px solid red;
}

.center {
  width: 50%;
  height: 50%;
  margin: 0 auto;
  /* margin: auto; */
  background-color: lightgrey;
  border: 1px dashed black;
}
```

- beware: overflowing boxes ignore their `auto` margins and overflow in the end direction, i.e. can't center ❗️



## Resources

<!-- ToDo: revisit once module spec covers margins more indepth, e.g. 'auto' value -->
- [W3C - CSS Box Model Module Level 4](https://www.w3.org/TR/css-box-4/)