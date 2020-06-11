<!-- ToDo: incorporate, then delete -->

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