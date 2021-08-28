---
title: Shadow
author: vwkd
index: 4.5
tags:
  - css
---
# Shadow



## Introduction

- drop-shadow(s) on box
- not part of box, doesn't affect layout or size ❗️
- beware: doesn't affect or increase scrollable area ❗️
- drawn directly behind / in front of background if outside / inside box, see Inset value
- beware: if outside box is like outline, except stacking order ❗️
- beware: if bigger than margin / padding may underlap into surroundings / contents ❗️



## Layers

- consist of one or more layers, like background
- one layer for each box shadow
- the property accepts comma-separated list of its value, i.e. can repeat value one or more times
- each value corresponds to a layer
- first value corresponds to foremost layer



## Shadow property

- specify one or more drop-shadows on box
- `box-shadow`
- not inherited



## Shadow values

- values: `<length>`{2,4} `<color>`? `inset`?
- sub-values can be in any order, e.g. `inset black 10px 10px`
- value can be repeated comma-separated on or more times, see Layers
- initial value: `none`
- percentage is not defined ⚠️
- beware: unlike for background, `none` is not a layer, disables entire shadow ❗️

### Length value

- first length: horizontal offset of shadow, positive is to right, defaults to `0`
- second length: vertical offset of shadow, positive is to bottom, defaults to `0`
- beware: directions are physical and not logical, i.e. can't specify independent of writing mode, see Writing Mode ⚠️
- can compute offsets from angle `a` and distance `d` using trigonometric formulas, i.e. `x = d*cos(a)`, `y = d*sin(a)`
- third length: blur radius of shadow, can not be negative, defaults to `0`
- fourth length: spread distance of shadow, defaults to `0`

### Color value

- color of shadow
- defaults to value of `color` property of element

### Inset value

- by default, shadow is outside of box outwards from border box
- if specified, shadow is inside box inwards from padding box



## Resources

- [W3C - CSS Backgrounds and Borders Module Level 3](https://www.w3.org/TR/css-backgrounds-3/)