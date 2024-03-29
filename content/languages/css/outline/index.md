---
title: Outline
author: vwkd
index: 4.6
tags:
  - languages
  - css
---

- like border around border box
- not part of box, doesn't affect layout or size ❗️
- drawn in front of box
- beware: if bigger than margin of box may overlap surroundings ❗️
- by default used for `:focus` state
- beware: don't disable for `:focus` without providing alternative highlighting mechanism, used for accessibility, e.g. using `all: initial` reset ⚠️



## Outline width

- specify thickness of outline area, like border width
- shorthand `outline-width` for all four sides
- beware: no longhand for each side like `border-*-width` ❗️
- not inherited
- values and initial value identical to `border-width`
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Outline style

- specify line style of outline area, like border style
- shorthand `outline-style` for all four sides
- beware: no longhand for each side like `border-*-style` ❗️
- not inherited
- values and initial value identical to `border-style`, except additional `auto` value and without `hidden` value
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Outline color

- specify color of outline area, like border color
- shorthand `outline-color` for all four sides
- beware: no longhand for each side like `border-*-color` ❗️
- not inherited
- values and initial value identical to `border-color`, except additional `invert` value
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Outline shorthands

- specify thickness, line style and color of outline area, like border shorthand
- shorthand `outline` combines `outline-*` for all four sides
- beware: resets omitted outline properties to their initial value ❗️
- beware: no shorthand for each side like `border-*` ❗️
- not inherited
- beware: user agent style sheet may set any value, overrides initial value, e.g. `:focus { outline: auto 1px -webkit-focus-ring-color; }` ❗️



## Outline offset

- specify offset of outline area outwards
- shorthand `outline-offset` for all four sides
- beware: no longhand for each side ❗️
- not inherited
- values: `<length>`
- can be negative, offsets outline inward instead
- percentage is not defined ⚠️
- beware: no `auto` value ❗️
- initial value: `0`
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Resources

- [W3C - CSS Basic User Interface Module Level 4](https://www.w3.org/TR/css-ui-4/)