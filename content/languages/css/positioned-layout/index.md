---
title: Positioned Layout
author: vwkd
index: 7
tags:
  - languages
  - css
---

- an irregular layout, can be used with any regular layout
- can use to position element anywhere on page, no matter regular layout of parent
- beware: can position elements off-screen, not reachable anymore for user ❗️
- beware: for sticky and fixed positioning assume screen media only, would be different for paged media ❗️

<!-- Demo: CSS/position -->



## `position` property

- specifies positioning scheme used
- not inherited
- values: `static`, `relative`, `absolute`, `sticky`, `fixed`
- initial value: `static`
- non-positioned elements: elements with `position` being `static`
- positioned elements: elements with `position` being not `static`
- positioned elements are stacked by default in front of non-positioned elements, see Stacking
- beware: here use term "absolutely positioned element" only for element with `position: absolute`, not with `position: fixed` ❗️
- beware: here use term "relatively positioned element" only for element with `position: relative`, not with `position: sticky` ❗️

### Static positioning

- no positioning scheme
- box is regularly laid out, i.e. follows FC it's in
- offset properties don't apply

### Relative positioning

- offset relative to own margin box at `static` location
- space is not taken up by surrounding boxes, as if box were in `static` location
- keeps interacting with surrounding boxes, as if box were in `static` location
- parent grows to contain, as if box were in `static` location
- beware: position can change since dependent on FC, e.g. wrap to next line if ODT is `inline` ❗️
- can use to shift position while still keeping interaction

### Sticky positioning

- like relative positioning except offset origin
- offset relative to nearest scrollport, see Overflow
- offset is applied only after box reaches location, i.e. box "sticks" on scroll
- offset is applied only within content box of containing block, i.e. scrolled away after reaches end of containing block
- offset is applied only if offset property is non-`auto`
- beware: offset property must be set to non-`auto` otherwise won't "stick", e.g. set to `0` ❗️
- beware: can't use padding in containing block to extend space since sticks only within content box, needs to use size properties of containing block or last child ❗️
- beware: still interacts with surrounding boxes unlike fixed positioning 🎉
- can use to stick on scroll while still keeping interaction, e.g. sticky headers

### Absolute positioning

- offset relative to containing block
- space is taken up by surrounding boxes, as if box weren't in box tree at all
- doesn't interact with surrounding boxes, as if box weren't in box tree at all
- parent doesn't grow to contain, as if box weren't in box tree at all ⚠️
- beware: to contain needs to custom size parent large enough, or use other layout type than Absolute positioning, e.g. Float Layout, Flexbox Layout, etc. ❗️
- beware: only use for single elements since can't position rest of elements relative to them anymore ❗️
- size becomes `max-content` by default, see Size#auto
<!-- ??? DEPENDING ON SELF-ALIGNMENT PROPERTIES -->
- beware: blockifies box ❗️
- `float` computes to `none`, i.e. overrides floating behavior in a flow FC ❗️
- can use to position anywhere on page

### Fixed positioning

- like absolute positioning
- containing block is usually viewport, i.e. box stays fixed on scroll, see Containing block
- beware: breaks page on page zoom since occupies more space, works with page scale only, see Viewport ⚠️⚠️⚠️
- beware: decreases scroll performance since browser needs to repaint layout instead of just shifting position of pixels ⚠️
- can use to position anywhere on screen, e.g. persistent navigation menu



## Offset properties

- specify location of margin box
- offset origin depends on positioning scheme, see above in `position` property
- apply only to positioned element
- beware: no effect on non-positioned elements ❗️
- physical properties: `top`, `left`, `right`, `bottom`, shorthand `inset` for all four
- logical properties: `inset-block-start`, `inset-block-end`, `inset-inline-start`, `inset-inline-end`, shorthands `inset-block` for `inset-block-*`, `inset-inline` for `inset-inline-*`, see Writing Mode
- beware: `inset` is physical shorthand for `top`, `bottom`, `left`, `right`, not a logical shorthand for `inset-*` ❗️
- beware: for grid item only use logical properties because offset origin is writing mode relative ⚠️
- not inherited
- values: `auto`, `<length-percentage>`
- initial value: `auto`
- percentage relative to _block size_/_inline size_ of containing block, see Writing Mode
- beware: don't confuse offset origin and `<percentage>` type in offset properties, `<percentage>` is relative to containing block in all positioning schemes (even `static`), offset origin only in absolute positioning ❗️
- start side: `top` in vertical axis, `left`/`right` in horizontal axis depending on if direction is `ltr`/`rtl`, see Writing Mode
- end side: opposite side of start side

### `auto`

- for relatively / stickily positioned element `auto` values on opposite sides behave as follows
  - if both sides are `auto`, both compute to zero, i.e. box stays in `static` location
  - if one side is `auto`, it computes to negation of other side, i.e. box is shifted
  - if both sides are non-`auto`, end side computes to `auto` since overconstrained, i.e. above case
- beware: size of a relatively / stickily positioned element is never changed ❗️
- for absolutely / fixedly positioned element `auto` values on opposite sides behave as follows
  - if both sides are `auto`, both compute such that size doesn't change, aligned according to self-alignment properties, i.e. box is shifted, by default start side aligned to `static` location
  <!-- ToDo: What are self-alignment properties? see https://www.w3.org/TR/css-align-3/#abspos-sizing -->
  - if one side is `auto`, it computes such that size doesn't change, i.e. box is shifted
  - if both sides are non-`auto`, if size is `auto` then box is resized, i.e. box is shifted and resized, if size is non-`auto` then end side computes to `auto` since overconstrained, i.e. above case
- beware: unchanged size is size before applying offset properties, i.e. for absolutely / fixedly positioned element `max-content` by default, see Size#auto ❗️
- beware: size of absolutely / fixedly positioned element is only changed when size is `auto` and both offset properties are non-`auto` ❗️
- beware: "`static` location" of absolutely / fixedly positioned element in Flex Layout is as if it were the sole flex item in the flex container ❗️

```css
p {
  position: absolute; /* fixed */
  width: 300px;
  left: 10px;
  /* end side ignored since width is explicitly set */
  right: 10px;
}
```

- beware: "`static` location" of absolutely / fixedly positioned element in Grid Layout is it's grid area if it has the grid container as containing block, otherwise as if it were the sole grid item in a grid area whose edges coincide with the content edges of the grid container ❗️



## Containing block

- ancestor box relative to which a descendant box is sized and positioned
- ancestor box depends on positioning scheme
- used for offset origin for absolutely / fixedly positioned elements
- used for `<percentage>` type in several box properties, e.g. padding, margin, size, offset properties, etc.
- beware: don't confuse offset origin and `<percentage>` type in offset properties, `<percentage>` is relative to containing block in all positioning schemes (even `static`), offset origin only in absolute positioning ❗️
- beware: even relevant outside of Positioned Layout for `<percentage>` type ❗️
- beware: should have been called "containing box", since not necessarily block-level element ❗️
- beware: a box is not confined by its containing block, can overflow ❗️
- beware: every box has a containing block, but not every box is a containing block ❗️
- beware: containing block is not necessarily parent ❗️
- beware: for grid item any calculations relative to the containing block are done relative to the grid area it's placed in even though it's not the containing block, e.g. offset origin for absolutely / fixedly positioned elements, `<percentage>` type, etc. ⚠️⚠️⚠️

### Static, relative, sticky positioning

- for flow item is content box of nearest ancestor box with ODT not inline
<!-- ??? no precise definition, spec uses old notion of FC that not every box established -->
- for flex item, grid item is content box of parent
- if no such ancestor exists, then viewport (in screen media) or the page area (in paged media)

### Absolute, fixed positioning

- nearest ancestor box that uses one of the properties `position` (only for absolute positioning), `transform`, `perspective`, `will-change`, `filter`, `contain`, etc. with certain values, see [MDN - Identifying the containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block#Identifying_the_containing_block)
- padding box if ancestor has not ODT inline
- content box if ancestor has ODT inline, if split into fragments then of first and last fragment
- if no such ancestor exists, then viewport (in screen media) or the page area (in paged media)
- beware: for fixed positioning usually is ICB ❗️
- beware: often positions box relatively but without any offset just to make CB for absolutely positioned descendants ❗️
- beware: if ancestor is inline-level and split across lines, end might be located before start ❗️
- beware: if ancestor has custom size and sizing box is content box (default), the CB is bigger by size of padding ❗️
- beware: if ancestor has custom size and sizing box is border box, the CB is smaller by size of border ❗️



## Resources

- [W3C - CSS Positioned Layout Module Level 3](https://www.w3.org/TR/css-position-3/)
- [MDN - Identifying the containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block#Identifying_the_containing_block)