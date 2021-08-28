---
title: Margin Collapse
author: vwkd
index: 6.2
tags:
  - css
---
# Margin Collapse



## Introduction

- historic bug of Flow Layout
- not in other regular layouts, e.g. Flexbox Layout, Grid Layout, etc.
- not in other irregular layouts, except also in relatively / stickily positioned elements, e.g. absolutely / fixedly positioned elements, Float Layout, etc.
- partly fixed by flow-root FC but not fully, see Adjacent siblings
- can't fully prevent, see Adjacent siblings
- was used to eliminate excess space, instead should have used new selectors
  - `myel ~ myel`: all myel except first of type
  - `* ~ myel`: all myel except if first child
  - `myel:not(:first-of-type)` / `myel:not(:last-of-type)`: all myel except first / last of type
  - `myel:not(:first-child)` / `myel:not(:last-child)`: all myel except if first / last child
- language should never impose a guess what's best for most users, increases complexity everywhere ⚠️
- CSS versioning should be introduced to allow removing such bugs, see Blog/A versioned Web.md ⚠️



## The collapse

- two adjoining margins can collapse to one single margin
- continues recursively, i.e. many adjoining margins can collapse to one single margin
- adjoining margins: margins that have nothing between them, e.g. text content, padding, border, etc.
??? height, or min-height
- only margins in block direction collapse, i.e. vertical margins (top & bottom) in horizontal writing mode
- beware: margins in inline direction don't collapse, i.e. horizontal margins (top & bottom) in horizontal writing mode ❗️
- only margins of block-level elements, not of inline-level elements, also not of root element
- only in flow(-root) FC
- beware: margin collapse happens even across FC boundaries, see Parent and first / last child and see Element itself ⚠️
- beware: flow-root FC fixes margin collapse across FC boundaries but not within FC, i.e. not Adjacent siblings ❗️
- beware: add comment to code if used to prevent margin collapse ❗️
- beware: don't use border to visualise margins of box, affects margin collapse, instead use outline or browser Developer Tools ❗️



## The cases

- depends on relationship of boxes in document tree
- also any combination due to recursiveness, e.g.
  - margins of only child collapse with both margins of parent
  - margin of child collapses with margin of parent and then with margin of grandparent
  - margin of child collapses with margin of parent and then with margin of adjacent sibling of parent
  - margins of empty child collapse themselves and then with margin of parent
  - margins of empty only child collapse themselves and then with both margins of parent
  - etc.
- beware: floats and absolutely / fixedly positioned element are treated as if they didn't exist, i.e. can be between elements without stopping margin collapse ❗️

### Adjacent siblings

- margins of adjacent siblings
- single margin is between them
- if both are positive, is largest of both (most positive)
- if one is negative, is sum of both
- if both are negative, is largest of both (most negative)

```html
<p>Margin collapse makes the space between the boxes only 1em (largest of 1em and 1em), instead of 1em + 1em = 2em.</p>

<div>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</div>

<div>Cras maximus augue et consequat porta.</div>
```

```css
div {
  margin: 1em 0;
  background-color: lightsalmon;
  border: 1px dashed red;
  /* no prevention or fix */
}
```

- can't prevent since margins are always adjoining
- beware: not fixed by flow-root FC ❗️
<!-- seems to be fixed by display: inline-block WHY??? -->

### Parent and first / last child

- block-start margin of parent and first child, block-end margin of parent and last child
- single margin is on parent, not on child, i.e. parent doesn't contain margin of child ❗️
- beware: even if parent margin is zero, i.e. parent never contains margin of child ⚠️
- if all are positive, is largest of all (most positive)
- if at least one is negative, is sum of largest positive and largest negative
- if all are negative, is largest of all (most negative)
- can prevent by putting something between margins, e.g. text content next to child in parent, or padding, border, etc. on parent
- can prevent only for end margin also by making parent block size non-zero, e.g. height, min-height, etc. on parent

```html
<div class="parent">
  <!-- text content -->
  <div class="child">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</div>
  <!-- text content -->
</div>
```

```css
.parent {
  margin: 1em 0;
  background-color: lightgrey;
  /* ---- prevention ---- */
  /* border: 1px dashed black; */
  /* padding: 1px 0; */
  /* ---- prevention bottom only ---- */
  /* height: 3em; */
  /* min-height: 3em; */
  /* ---- fix ---- */
  /* display: flow-root; */
  /* ---- legacy fixes ---- */
  /* overflow: auto; */
  /* float: left; */
  /* position: absolute; */
  /* position: fixed; */
}

.child {
  margin: 2em 0;
  background-color: lightsalmon;
  border: 1px dashed red;
}
```

- fixed with flow-root FC 🎉
- beware: don't use legacy hacks anymore, see legacy hack 2 and 4 (3 anyways) in Float Layout#Containing floats ❗️

### Element itself

- own margins of element itself
- single margin is on top
- if both are positive, is largest of both (most positive)
- if one is negative, is sum of both
- if both are negative, is largest of both (most negative)
- beware: element must be empty of text content, otherwise would have something between margins ❗️
- can prevent by putting something between margins, e.g. padding, border, etc., or making block size non-zero, e.g. height, min-height, etc.

```html
<div>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</div>

<div class="empty">
  <!-- text content -->
</div>

<div>Cras maximus augue et consequat porta.</div>
```

```css
.empty {
  margin: 1em 0;
  background-color: lightsalmon;
  /* ---- prevention ---- */
  /* border: 1px dashed red; */
  /* padding: 1px 0; */
  /* height: 1px; */
  /* min-height: 1px; */
  /* ---- fix ---- */
  /* display: flow-root; */
  /* ---- legacy fixes ---- */
  /* overflow: auto; */
}
```

- fixed with flow-root FC 🎉
- beware: don't use legacy hacks anymore, see legacy hack 2 (3 anyways) in Float Layout#Containing floats ❗️
- beware: empty box must establish flow-root FC, not be in flow-root FC ❗️



## Resources

<!-- ToDo: revisit once covered by module spec, e.g. https://www.w3.org/TR/css-box-4/ -->
- [W3C - CSS2.2 8.3.1 Collapsing margins](https://www.w3.org/TR/CSS22/box.html#collapsing-margins)