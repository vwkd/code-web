<!-- ToDo: incorporate, then delete -->

++——————————————————————————————————++

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