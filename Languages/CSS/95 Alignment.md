# Alignment

[TOC]


<!-- ToDo: replace in Grid and Flex Layout -->



## box alignment properties

dimension they apply to (main/inline vs. cross/block)
control the position of the box within its parent, or the box’s content within itself
beware: “justify” and “align” to distinguish between alignment in the main/inline and cross/block dimensions, arbitrary definition

justify/align-content
content within element
applies to flex containers, grid containers
in grid layout: means grid columns/rows ???
in flex layout: means flex items / flex lines ???
  SHOULD HAVE BEEN FLEX LINE ITSELF FOR justify-content!!!
direction according to writing mode in that element

justify/align-items
elements within content
  aligns margin box of elements
shorthand to set all child items with justify-self: auto
  specify the interpretation of any *-self: auto used on children of the container
applies to flex containers (only align!), grid containers
in grid layout: means grid items within columns/rows ???
in flex layout: means flex items within flex lines ???
direction according to writing mode in that element
  I.E. FOR CHANGING FLEX-DIRECTION THE DON'T CHANGE SINCE WRITING MODE DOESN'T CHANGE ??? OR DOES CHANGE
  I.E. IN GRID LAYOUT FOR CHANGING WRITING-MODE DOESN'T CHANGE ??

justify/align-self
element within content
applies to flex items (only align!), grid items


## alignment terminology

### targets

alignment subject: thing(s) being aligned
alignment container: rectangle the alignment subject is aligned within
  usually the alignment subject’s containing block

### keywords

#### positional alignment

absolute position in alignment container


center, start, end, self-start, self-end, flex-start, flex-end, left, and right keywords



#### baseline alignment


#### distributed alignment


justify/align-content
alignment subject: varies, defined by the layout mode

justify/align-self
alignment subject: margin box of element that property is set on



----- OLD

position of the box within its parent, or the box’s content within itself.


arbitrary “justify” and “align” to distinguish between alignment in the main/inline and cross/block dimensions


The *-items properties don’t affect the element itself. When set on a container, they specify the interpretation of any *-self: auto used on children of the container element.


## Resources

- [W3C - CSS Box Alignment Module Level 3](https://www.w3.org/TR/css-align-3/)