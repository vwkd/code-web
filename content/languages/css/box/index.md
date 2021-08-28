---
title: Box
author: vwkd
index: 4
tags:
  - css
---
# Box



## Introduction

- visual representation of element in a document
- every element is represented as one (principal) box
- style of box is taken from properties of element
- document tree is transformed into box tree
- box layout defines how box tree is rendered
- box tree hierarchy is same as document tree hierarchy, may have additional boxes for layout
- term "a box contains another box", if in the box tree it has the box as a descendant
- term "descendant box" refers to box of descendant element
- see Layout and Layout#Box_generation
- beware: principal box is often used as synonym for element itself, e.g. box establishes FC, etc. ⚠️



## Box

- rectangular content area with optional padding, border, and margin areas

![box](static/box.svg)

- can think of box as consisting of four nested sub-boxes, i.e. content box, padding box, border box, and margin box
- content area contains the elements content, e.g. text, image, descendant elements, etc.  
- think of total box as border box
- don't think of margin as part of box, see Margin ❗️
- beware: outside of Flow Layout margin isn't treated as part of box, e.g. background doesn't extend into margin, pointer events aren't registered on margin, etc. ⚠️
- box size by default automatically adapts to fit content, see Size

<!-- TODO see css-break-4
a box can "break" into fragments, e.g. end of line, end of page on print, etc.
e.g. inline-level box which establishes a flow FC and has a block-level child box

Margins adjoining a fragmentation break are sometimes truncated, e.g. on print
-->



## Resources

- [W3C - CSS Box Model Module Level 4](https://www.w3.org/TR/css-box-4/)