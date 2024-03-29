---
title: Border
author: vwkd
index: 4.2
tags:
  - languages
  - css
---

- painted area outside of padding
- drawn above background



## Border width

- specify thickness of border area
- longhand `border-*-width` for each side, shorthand `border-width` for all four sides
- not inherited
- values: `<length>`, `thin`, `medium`, `thick`
- keywords are specified by user agent
- can not be negative
- percentage is not defined ⚠️
- beware: no `auto` value ❗️
- initial value: `medium`
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Border style

- specify line style of border area
- longhand `border-*-style` for each side, shorthand `border-style` for all four sides
- not inherited
- values: `none`, `hidden`, `dotted`, `dashed`, `solid`, `double`, `groove`, `ridge`, `inset`, `outset`
- beware: no `auto` value ❗️
- beware: `none` makes border not appear, color and width are ignored ❗️
- initial value: `none`, i.e. no border by default ❗️
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Border color

- specify color of border area
- longhand `border-*-color` for each side, shorthand `border-color` for all four sides
- not inherited
- values: `<color>`
- beware: no `auto` value ❗️
- initial value: value of `color` property
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Border shorthands

- specify thickness, line style and color of border area
- shorthand `border-*` combines `border-*-width`, `border-*-color` and `border-*-style` for each side, in any order
- shorthand `border` combines `border-*` for all four sides
- beware: `border` can't set different values for each side, unlike `margin` and `padding` ❗️
- beware: resets omitted border properties to their initial value, including `border-image` ❗️
- not inherited
- beware: user agent style sheet may set any value, overrides initial value, e.g. `hr { border: 1px inset; }` ❗️



## Border radius

- specify semi-major and -minor axes of ellipse of corner curvature of outer edge of border area
- inner edge makes smooth transition for any difference in line thickness
- beware: if border radius is smaller than thickness of both lines, inner edge is not curved ❗️
- longhand `border-*-radius` for each corner, shorthand `border-radius` for all four corners
- not inherited
- values: `<length-percentage>`
- can not be negative
- percentage relative to _block size_/_inline size_ of containing block, see Writing Mode
- beware: no `auto` value ❗️
- initial value: `0`
- beware: user agent style sheet may set any value, overrides initial value, e.g. `select { border-radius: 5px; }` ❗️

### Length value

- first value is horizontal axis, second is vertical axis
- single value is for both axes
- if one is zero, other computes to zero as well, i.e. no curvature ❗️
- beware: directions are physical and not logical, i.e. can't specify independent of writing mode, see Writing Mode ⚠️
- beware: shorthand accepts any values for horizontal axis, then after `/` any values for vertical axis ❗️
- beware: sum of adjacent border radii can't exceed size of box in same spacial dimension otherwise lines wouldn't meet, larger values are reduced by same amount until sum is maximal ❗️

```html
<div class="box"></div>
```

```css
.box {
  box-sizing: border-box;
  width: 600px;
  height: 200px;
  border: 1px solid black;
  /* computes to 40px 160px 40px 160px; */
  border-radius: 50px 200px 50px 200px;
}
```

- beware: percentages refer to different values for non-square box, i.e. must use non-percentage value for circular corners ❗️

```html
<div class="box"></div>
```

```css
.box {
  box-sizing: border-box;
  width: 600px;
  height: 200px;
  border: 1px solid black;
  /* elliptical, since width != height */
  border-radius: 50%;
  /* circular */
  border-radius: 50px;
  /* as circular as possible, since 2*100px = min(height, width) */
  border-radius: 100px;
  /* as circular as possible, since reduced to maximal value */
  border-radius: 999px;
}
```



## Resources

- [W3C - CSS Backgrounds and Borders Module Level 3](https://www.w3.org/TR/css-backgrounds-3/)