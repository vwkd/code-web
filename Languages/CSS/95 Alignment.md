# Alignment

[TOC]


<!-- ToDo: replace in Grid and Flex Layout -->



## box alignment properties

dimension they apply to (main/inline vs. cross/block)
control the position of the box within its parent, or the box’s content within itself
beware: “justify” and “align” to distinguish between alignment in the inline axis (main axis in Flex Layout) and block axis (cross axis in Flex Layout), arbitrary definition
  i.e. GL justify- aligns columns, align- aligns rows
beware: for flex-direction: column(-reverse) main doesn't correspond to inline axis (and cross not to block axis) !!!!!!

### Content Distribution

aligns grid tracks in appropriate axis within content box of element
i.e. effectively adjusts padding of element

aligns flex lines (align) or flex items in each line (justify) within content box of element
i.e. effectively adjusts padding of element
beware: should have used flex-items for behavior of justify-content !!!
beware: align-content only has effect on multi-line flex containers !
NOTE: stretch behaves as flex-start.

justify/align-content, place-content shorthand
applies to flex containers, grid containers
direction according to writing mode in that element ?? OR MEANDS WRITING DIRECTION

align-content
normal |  <content-distribution> | <overflow-position>? <content-position>
initial value: normal, behaves as stretch
applies to flex containers, grid containers
not inherited

justify-content
Value: normal | <content-distribution> | <overflow-position>? <content-position>
initial value: normal, behaves as stretch
applies to flex containers, grid containers
not inherited

shorthand for both
place-content
Value: <'align-content'> <'justify-content'>?
Initial: normal
Applies to: block containers, flex containers, and grid containers
Inherited: no
if second is ommitted it's copied from the first value

alignment subject and alignment container both assume the writing mode of the box the *-content property is set on ??? WHAT DOES THIS MEAN

<!-- todo: overflow, see 5.3, css-overflow -->
<!-- todo: baseline, see 4.2, 5.4, 9, css-align -->

### Self-Alignment

aligns margin box of elements within content ??? within its containing block
i.e. effectively adjusts margins of elements


justify/align-items, place-self shorthand
shorthand to set all child items with justify-self: auto
  specify the interpretation of any *-self: auto used on children of the container
applies to flex containers (only align!), grid containers
in grid layout: means grid items within columns/rows ???
in flex layout: means flex items within flex lines ???
direction according to writing mode in that element ?? OR MEANDS WRITING DIRECTION

justify/align-self
like -items for individual item only
applies to flex items (only align!), grid items


#### justify-self

Value: auto | normal | stretch | <overflow-position>? <self-position>
Initial: auto
Applies to: absolutely-positioned boxes, and grid items
Inherited: no

auto behaves as
  the computed justify-items value of the parent box
  OR as normal if the box has no parent, or when determining the actual position of an absolutely positioned box

normal behaves as IN GRID LAYOUT
Effectively stretch for items with no intrinsic aspect ratio, and start for items with an intrinsic aspect ratio: see Grid Item Sizing in [CSS-GRID-1]. The resulting box is start-aligned.

stretch: 
When the box’s computed width/height (as appropriate to the axis) is auto and neither of its margins (in the appropriate axis) are auto, sets the box’s used size to the length necessary to make its outer size as close to filling the alignment container as possible while still respecting the constraints imposed by min-height/min-width/max-height/max-width.
Unless otherwise specified, this value falls back to flex-start
Note: The stretch keyword can cause elements to shrink, to fit their container.

FL: See flex for stretching and justify-content for main-axis alignment.

<!-- todo: abs pos boxes, Appendix A: Static Position Terminology -->
<!-- todo: baseline, 9, see css-align -->

#### align-self

Value: auto | normal | stretch | <overflow-position>? <self-position>
Initial: auto
Applies to: flex items, grid items, and absolutely-positioned boxes
Inherited: no

LIKE JUSTIFY-SELF

normal behaves as stretch. IN FLEX LAYOUT
  for non-replaced items. For replaced items, see Grid Item Sizing in [CSS-GRID-1].

<!-- todo: abs pos boxes -->
<!-- todo: baseline, 9, see css-align -->


#### place-self

shorthand for align-self and justify-self

Value: <'align-self'> <'justify-self'>?
Initial: auto
Applies to: block-level boxes, absolutely-positioned boxes, and grid items
Inherited: no

if second value is omitted, it is copied from the first value


<!-- todo: 6.5. Effects on Sizing of Absolutely Positioned Boxes with Static-Position Insets -->





### Default Alignment

set the default align-self and justify-self behavior of the element’s child boxes I.E. THOSE WITH `auto`


## alignment terminology

### targets

alignment subject: thing(s) being aligned
alignment container: rectangle the alignment subject is aligned within
  usually the alignment subject’s containing block

### keywords

#### positional alignment (in all)

self-alignment: (-self, -items)
center, start, end, self-start, self-end, flex-start, flex-end
<self-position>
start and end: use the writing mode of alignment container
self-start/-end: use writing mode of alignment subject
flex-start/end: ??
layout modes other than flex layout, flex-start is identical to start.

content-distribution: (-content)
center, start, end, flex-start, flex-end
<content-position>
can align overflow with safe and unsafe modifiers
<overflow-position>

#### distributed alignment (only in -content)

stretch, space-between, space-around, space-evenly
<content-distribution>

space-between: default fallback alignment for this value is flex-start

space-around, space-evenly: default fallback alignment for this value is center

stretch:
any auto-sized alignment subjects have their size increased equally (not proportionally) to fill the alignment container
still respecting the constraints imposed by max-height/max-width (or equivalent functionality)
default fallback alignment for this value is flex-start