---
title: Background
author: vwkd
index: 4.4
tags:
  - languages
  - css
---

- painted layer(s) behind box
- drawn below border
- beware: solid border style overlays background, needs to make opaque, dashed, etc. to see background ❗️
- beware: properties should have been named better, e.g. `background-origin`, `background-attachment`, etc. ⚠️
- beware: here misuse term "size" for the length in single one of the two spacial dimensions, see NFP/Size ⚠️



## Layers

- consist of one or more layers
- one layer for each background image
- beware: background color is not counted as layer ❗️
- properties accept comma-separated list of their value, i.e. can repeat value one or more times
- each value corresponds to a layer
- first value corresponds to foremost layer
- list with less values than layers is repeated until long enough
- list with more values than layers is truncated
- background color is drawn below any layers
- background color can't be customised by other properties except clip property, e.g. origin, position, etc.

```css
myel {
  /* three layers */
  background-image: url(a.png), url(b.png), url(c.png);
  background-color: green;
  /* computes to: center center, 20% 80%, top left; */
  background-position: center center, 20% 80%, top left, bottom right;
  /* computes to: border-box, content-box, border-box; */
  background-origin: border-box, content-box;
  /* computes to: no-repeat, no-repeat, no-repeat; */
  background-repeat: no-repeat;
}
```



## Color

- specify color behind all background layers
- `background-color`
- not inherited
- values: `<color>`
- initial value: `transparent`
- beware: no `auto` value ❗️
- beware: user agent style sheet may set any value, overrides initial value, e.g. `mark { background-color: yellow; }` ❗️
- beware: background of parent element shines through box by default since transparent ❗️



## Image

- specify image for each background layer
- `background-image`
- not inherited
- values: `<image>`, `none`
- initial value: `none`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️
- beware: `none` creates a transparent layer, i.e. still creates a layer ❗️

```css
myel {
  background-image: url(a.png), none, url(b.png);
  /* url(b.png) gets border-box instead of content-box */
  background-origin: border-box, content-box, border-box;
}
```

```css
myel {
  background-image: url(a.png), none;
  background-color: lightgrey;
  /* clips background-color but not url(a.png) */
  background-clip: border-box, content-box;
}

```

- beware: set color as fallback in case image(s) aren't available, e.g. download fails, unsupported format, etc. ❗️



## Painting area

- specify painting area of image for each background layer
- `background-clip`
- not inherited
- values: `border-box`, `padding-box`, `content-box`
- initial value: `border-box`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️
- beware: clip of background color is copied from bottom-most layer, i.e. last value of `background-clip` ❗️



## Positioning area

- specify positioning area of image for each background layer
- `background-origin`
- not inherited
- values: `border-box`, `padding-box`, `content-box`
- initial value: `padding-box`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️
- beware: if painting area is smaller than positioning area, image may be clipped ❗️

```html
<div class="box"></div>
```

```css
.box {
  width: 300px;
  height: 300px;
  padding: 30px;
  border: 30px dashed black;
  background-image: url(https://interactive-examples.mdn.mozilla.net/media/examples/lizard.png),
    none;
  background-color: lightgrey;
  background-clip: content-box;
  background-origin: border-box;
  background-repeat: no-repeat;
  background-size: 100%;
}
```

- beware: if painting area is larger than positioning area, image might extend further than started ❗️

```html
<div class="box"></div>
```

```css
.box {
  width: 300px;
  height: 300px;
  padding: 30px;
  border: 30px dashed black;
  background-image: url(https://interactive-examples.mdn.mozilla.net/media/examples/lizard.png),
    none;
  background-color: lightgrey;
  background-clip: border-box, content-box;
  background-origin: content-box;
  background-repeat: no-repeat;
  background-size: 120%;
}
```



## Initial position

- specify initial position of image in positioning area for each background layer
- `background-position`
- not inherited
- values: `<position>`
- initial value: `0% 0%`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️
- beware: for non-canonical origin, positive axis directions are inwards to positioning area, see Data Types#Position ❗️



## Initial size

- specify initial size of image for each background layer
- `background-size`
- not inherited
- values: (`<length-percentage>`, `auto`){1,2}, `cover`, `contain`
- first value is for width, second for height
- single value is for width, height computes to `auto`
- can not be negative
- percentage relative to width / height of positioning area
- beware: not relative to inline size / block size ❗️
- initial value: `auto`, i.e. `auto auto`
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️

### `auto`

- if one side is `auto`, it computes to value that preserves intrinsic aspect ratio (if any), otherwise computes to intrinsic size (if any), otherwise computes to `100%`
- if both sides are `auto`, they compute to intrinsic size (if any), otherwise computes to `contain`
- beware: by default image is original size and not contained ❗️

### `cover`

- image is scaled to smallest size such that covers positioning area in both directions
- intrinsic aspect ratio is preserved (if any)
- always bigger than positioning area, i.e. may be clipped ❗️

### `contain`

- image is scaled to largest size such that still fits inside positioning area in both directions
- intrinsic aspect ratio is preserved (if any)
- always smaller than positioning area, i.e. may have gaps ❗️



## Tiling

- specify tiling of image for each background layer
- image is repeated in positive axis directions from initial position and initial size over whole painting area
- beware: if painting area is bigger than positioning area, tiling is continued in negative x- and y-axis direction as well ⚠️
- beware: if painting area differs from positioning area, image may be clipped, see Positioning area ⚠️
- `background-repeat`
- not inherited
- values: (`repeat`, `space`, `round`, `no-repeat`){1,2}, `repeat-x`, `repeat-y`
- first value is for horizontal direction, second for vertical
- single value is shorthand for both directions, e.g. `repeat` computes to `repeat repeat`
- shorthand `repeat-x` computes to `repeat no-repeat`, `repeat-y` computes to `no-repeat repeat`
- initial value: `repeat`, i.e. `repeat repeat`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️

### `repeat`

- image is repeated until covers painting area
- may be clipped
- no spacing between repetitions

### `space`

- image is repeated as often as possible in positioning area without being clipped
- repetitions are then spaced out equally until cover painting area
- beware: if painting area is smaller than positioning area can still clip image ❗️

### `round`

- like `space`, but image is rescaled to cover positioning area a round number of times
- rescaled up or down as little as possible, i.e. scaled size `X' = W / round(W / X)` for current size `X` and size of positioning area `W`
- beware: current size is controlled by `background-size` ❗️

### `no-repeat`

- image is drawn once



## Positioning scheme

- specify positioning scheme of image for each background layer
- `background-attachment`
- not inherited
- values: `scroll`, `fixed`, `local`
- initial value: `scroll`
- beware: no `auto` value ❗️
- value can be repeated comma-separated on or more times, see Layers
- beware: user agent style sheet may set any value, overrides initial value ❗️

### `local`

- image is fixed to scrollable area of element
- scrolls with element in parent and with contents in element
- beware: painting and positioning area are relative to scrollable area of element as well ❗️

### `scroll`

- image is fixed to viewport of element
- scrolls with element in parent but not with contents in element
- beware: painting and positioning area are relative to viewport of element as well ❗️

### `fixed`

- image is fixed to viewport
- neither scrolls with element in parent nor with contents in element
- beware: painting and positioning area are relative to viewport as well ❗️
- can use to create parallax scroll effect

<!-- Demo: CSS/background-attachment -->



## Shorthand

- specify all background properties for each background layer
- `background`
- order of values doesn't matter, except first `background-origin` then `background-clip` and single value is assigned to both, and value for `background-size` can follow only immediately after the value for `background-position` separated by a `/`
- beware: can't set `background-size` without setting `background-position` ❗️
- beware: resets omitted background properties to their initial value ❗️
- not inherited
- value can be repeated comma-separated on or more times, see Layers
- beware: `background-color` may only be in last value of list ❗️
- beware: user agent style sheet may set any value, overrides initial value ❗️



## Canvas

- infinite region in which the document is rendered
- background of canvas uses background of root element, i.e. background bleeds out, but not rest of element, e.g. border ❗️
- painting area of root element extends to cover entire canvas, positioning area stays confined to root element
- beware: `background-clip` has no effect on root element ❗️
- in HTML, if the root element `<html>` has no background, the background of the `<body>` element is used instead ❗️



## Resources

- [W3C - CSS Backgrounds and Borders Module Level 3](https://www.w3.org/TR/css-backgrounds-3/)