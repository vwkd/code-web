# CSS Layout

++——————————————————————————————————++
++——————————————————————————————————++

## The box model

### The box
(img)
- each HTML element is like a box, has content, padding, border and margin
- the box resizes dynamically based on content (if not fixed by styling)
- the margin is always invisible and separates the element from its neighbors
- for adjacent elements horizontal margins add, vertical margins collapse to only the larger margin of both margins (except for floating and absolutely positioned elements)
- can visualize boxes while developing, but since uses borders themselves can only show padding-boxes and only if no separate borders have been defined
* {
	border: 1px solid red;
}

### Box styling

\[Box styling go here: Content, Padding, Border, Margin]

- sides using 4 arguments: top, right, bottom, left
- sides using 3 arguments: top, left/right, bottom
- sides using 2 arguments: top/bottom, left/right
- sides using 1 argument: all

- `box-sizing`: specifies how size of element is calculated
	`content-box` (default, size includes only content), `border-box` (size includes content, padding and border, i.e. shrinks content box)

###  Box types

##### Block-level elements
- Can contain other inline- or block-level elements
- CSS properties `height`, `width`, `text-align` have effect
- By default take up full width of parent element, with its padding, border and margins are already included in calculation
- By default start in new line, flow is top to bottom
- Examples: headings, `<p>`, lists and `<li>`, `<table>`, `<div>`

##### Inline-level elements
- Can contain only other inline-level elements
- CSS properties `height`, `width`, `text-align` have no effect 
- By default take up only as much width as needed of parent element
- By default start in-line, flow is left to right
- Examples: `<a>`, `<img>`, most formatting elements, `<span>`

### HTML flow
- HTML elements are positioned as a flow of boxes, one box after the other
- In western writing mode (horizontal-tb) block-level elements flow from top to bottom, inline elements flow from left to right and wrap into the next line
- The normal flow pushes the boxes dynamically to accommodate all
- Use HTML elements according to their semantic meaning, not their default position, the position can always be changed later

++——————————————————————————————————++
++——————————————————————————————————++

## Layout tools

CSS Grid and Flexbox.
- Floats — Applying a float value such as left can cause block level elements to wrap alongside one side of an element, like the way images sometimes have text floating around them in magazine layouts.

- Table layout — features designed for styling the parts of an HTML table can be used on non-table elements using display: table and associated properties.
- Multi-column layout — The Multi-column layout properties can cause the content of a block to layout in columns, as you might see in a newspaper.
- !!! Don’t use tables to layout, very messy

++——————————————————————————————————++

### `display` property

- specified the display behavior of an element
- changes the behavior of an element to that of a different box type (but not nature itself!), e.g. can make `<span>` look like block-level element but still can’t nest `<div>` inside it
- `none`: removes element from view entirely, to only hide but keep space use `visibility` property
- `block`: box of a block-level element, i.e. has size properties
- `inline`: box of a inline-level element , i.e. has no size properties
- `inline-block` box of a block-level element in line, i.e. has size properties

++——————————————————————————————————++

### `position` property

- specifies the positioning method used for an element
- takes an element out of the normal flow so it can be positioned using offsets `top`, `right`, `bottom`, `left`
- an element that is out of flow isn’t „known“ to the surrounding elements, it leaves no gap and behaves completely independently, for example margins do not collapse with margins from elements in flow
- an element is said to be positioned if its position is not `static`
- for all elements, not only block-level ??? GIVES INLINE A WIDTH???

- `static` (default): element position is determined by normal flow, in flow
- `relative`: element position is relative to its position in normal flow, leaves gap where it was in normal flow as if it wasn’t changed
 - `absolute`: position of element is relative to its _containing block_, out of flow
- `fixed`: position of element is relative to viewport, out of flow
- `sticky`: like `relative` until it reaches specified offset, then like `fixed`, works only over the area of the parent element

#### `top`, `right`, `bottom`, `left`  properties

- specify the position of a „positioned“ element, i.e. position is not `static`
- if height and top is specified, bottom has no effect, similarly if width and left is specified, right has no effect, the other way around in right-to-left mode

- `auto`: default, do nothing
- \[length]: offset, according to `position`
- %: offset, relative to element’s containing block, even if `position:relative`

#### Containing block

The containing block of an element is the
- content-box of nearest _block-level_ ancestor, if position of element is `static`, `relative` or `sticky` (normal case)
- padding-box of nearest _positioned_ ancestor, if position of element is `absolute`
- viewport, if position of element is `fixed`
- (padding-box of nearest ancestor with a transform or perspective property, if position of element is `fixed` or `absolute`)

++——————————————————————————————————++

### ??? Floating elements

#### `float` property

- direction towards which an element floats within parent element
- `none` (default), `left`, `right`
- ??? not only for block elements???? no…
- ??? needs to specify size ??? will take on size of content only ?!
- content of surrounding elements float around the floating element but not elements themselves ?????????????
- multiple floating elements float in the order of appearance, if size becomes to small will wrap to new line
- ??? parent doesn’t adjust size to fit floating element
- ??? if float is larger than parent will overflow, can fix with `overflow:auto` on parent but might show scrollbars ???? better use
	.clearfix::after {
	  content: "";
	  clear: both;
	  display: table;
	}

#### `clear` property

- direction(s) on which floating elements are not allowed
- `none` (default), `left`, `right`, `both`
- commonly used on elements that should not wrap around floating element anymore, clears the float after use, clears towards same sides or both


	coupled with width property, if width: 100% no space for floats anyway


++——————————————————————————————————++

### ??? Flexbox

auf container
- display: flex;
-   flex-wrap: nowrap;

++——————————————————————————————————++


## Stacking

#### Stack Order
- elements are stacked in order of appearance within their categories
- non-positioned elements are stacked above root background and border
- floating blocks are stacked above non-positioned elements
- positioned elements are stacked above floating blocks
- Note: inline elements in non-positioned elements are beside floating elements

#### `z-index`
- create custom stacking order for positioned elements, „stacking context“
- a stacking context is created by an element with a specified `z-index` for its descendants, the first stacking context is created by the root element
- a stacking context is completely independent of its siblings
- a stacking context is stacked according to the `z-index` of its creating element
- `z-index`: auto (default, no custom stacking order), \[integer]
- element with greater `z-index` is placed in front of lower `z-index`
- for same `z-index` the normal stack order applies

++——————————————————————————————————++



- beware, that here % is relative to the element’s containing block which is not necessarily the element’s parent



## ??? Alignment

### Horizontal

#### Centering

- `text-align:center` for text
- `width:FIX` + `margin: FREE auto` for block-level element
- `width:FIX` + `margin: FREE auto` + `position:absolute`, `left`/`right:0` for block-level element 

#### Left / Right

- `text-align:left`/`right` for text
- `width:FIX` + `margin-left:auto`, `margin-right:0` (or reverse) only for block-level element
- `width:FIX` + `position:absolute` + `right:0` (or reverse)
- `width:FIX` + `float:left`/`right` + clearfix

### Vertical

#### Centering

- `padding:FIX FREE`
- `height:FIX` + `line-height:FIX` (same as width)
- `height:FIX` + `margin: auto FREE` + `position:absolute`, `top`/`bottom:0`
- `position:absolute`, `top`/`bottom:50%` + `transform:translate(0,-50%)` ????????

#### Top / Bottom

- `padding-top`/`padding-bottom:FIX`
- `height:FIX` + `position:absolute`, `top`/`bottom:0`

- margin:auto + position:flex ??
- ?? grid view, flexbox

??????????????????????
horizontal menu

with display:inline(-block) or float, but wraps around line one by one if screen size goes down