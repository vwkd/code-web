---
title: Padding
author: vwkd
index: 4.1
tags:
  - languages
  - css
---

- transparent area between content and border
- spaces content from border



## Padding properties

- specify thickness of padding area
- longhand `padding-*` for each side, shorthand `padding` for all four sides
- not inherited



## Padding values

- values: `<length-percentage>`
- can not be negative
- initial value: `0`
- beware: no `auto` value ❗️
- percentage relative to _inline size_ of containing block, i.e. _width_ in horizontal writing mode, see Writing Mode
- beware: even for `padding-block-start` and `padding-block-end`, allows same value to make equal sized padding in any direction ❗️
- beware: user agent style sheet may set any value, overrides initial value, e.g. `ul, ol { padding-inline-start: 40px; }` ❗️



## Resources

- [W3C - CSS Box Model Module Level 4](https://www.w3.org/TR/css-box-4/)