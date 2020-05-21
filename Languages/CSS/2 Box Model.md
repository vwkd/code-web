# Box model

[TOC]



## Introduction

- rendering of elements in a document
- every element is rendered as a box, principal box
- document tree is transformed into box tree
- style of box is taken from CSS properties of element
- additional helper boxes are generated for some elements, will only look at principal box here



## Viewport

- area where document can be viewed, i.e. browser window excluding menu bar, etc.
- user agent offers scrolling mechanism if viewport is smaller than rendered document
- user agent adapts layout of document to viewport size automatically, e.g. text wrap ???
- 2D coordinate system, origin in top left corner



## Box

- rectangular content area with optional padding, border, and margin areas

![box](box.svg)

- can think of box as four boxes in one, content box, padding box, border box, margin box (beware: don't confuse with descendant boxes)
- content area contains the boxes content, e.g. text, image, descendant boxes, etc.



## Box Layout

- visual arrangement of boxes
- multiple layout models, e.g. flow layout, flex layout, grid layout, table layout, positioned layout
<!-- ToDo: write multiple layout models -->

<!-- TODO see css-sizing-3
- width / height of the box refers to width / height of content area
CAN CHANGE USING BOX-SIZING???

size of box often depends on the element’s content and/or its containing block size
The sizing properties, together with various other properties that control layout, define the size of the content area
-->

<!-- TODO see css-break-4
a box can "break" into fragments, e.g. end of line, end of page on print, etc.
-->



## Margin

- visually separates boxes

- can control using ?

margin properties specify the thickness of the margin area of a box. The margin shorthand property sets the margin for all four sides while the margin longhand properties only set their respective side

Adjoining margins in block layout collapse, see Block Layout
Margins adjoining a fragmentation break are sometimes truncated
<!-- ToDo: see css-break-4 -->

Negative values for margin properties are allowed, but there may be implementation-specific limits.
- only margins can be negative, effectively sizing the box ???

initial value is 0
percentage refers to width of containing block

## Padding

- visually separates content from border

padding properties specify the thickness of the padding area of a box. The padding shorthand property sets the padding for all four sides while the padding longhand properties only set their respective side


## Border

border properties specify the thickness of the border area of a box, as well as its drawing style and color.
<!-- ToDo: see css-background-3 -->

Borders fill the border area

## Background

- `background` of element styles background of content, padding and border area of box
- also `border` of element styles background of border area of box
- background of margin area is always transparent, i.e. background of outer box shines through

background of element box by default within the padding edges
can be adjusted using the background-origin and background-clip properties