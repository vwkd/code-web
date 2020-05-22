# CSS Measurements

- viewport (a window or other viewing area on the screen)
	doesn’t consider scrollbar that might take a few pixels

dimension = number with a unit attached to it
numeric data types include <integer>, <number>, <percentage>, and various dimensions including 


### Percentage

Percentage values are always relative to another quantity, for example a length. Each property that allows percentages also defines the quantity to which the percentage refers. This quantity can be a value of another property for the same element, the value of a property for an ancestor element, a measurement of the formatting context (e.g., the width of a containing block), or something else

- usually percentage is calculated relative to width or height of containing block
beware: e.g. percentage for `margin` is relative to _width_ of containing block, even for top and bottom margin ❗️
see definition table


### Distance unit, Length

#### Relative Lengths

unit	relative to
em	font size of the element
ex	x-height of the element’s font
ch	width of the "0" (ZERO, U+0030) glyph in the element’s font
rem	font size of the root element
vw	1% of viewport’s width (initial containing block.)
vh	1% of viewport’s height
vmin	1% of viewport’s smaller dimension
vmax	1% of viewport’s larger dimension
However, any scrollbars are assumed not to exist.

#### Absolute Lengths

unit	name	equivalence
cm	centimeters	1cm = 96px/2.54
mm	millimeters	1mm = 1/10th of 1cm
Q	quarter-millimeters	1Q = 1/40th of 1cm
in	inches	1in = 2.54cm = 96px
pc	picas	1pc = 1/6th of 1in
pt	points	1pt = 1/72th of 1in
px	pixels	1px = 1/96th of 1in


All of the absolute length units are compatible, and px is their canonical unit.
For screen media, these dimensions are anchored by relating the pixel unit to the reference pixel (i.e. the anchor unit be the pixel unit)
For screens, pixel unit refer to the whole number of device pixels that best approximates the reference pixel
If the anchor unit is the pixel unit, the physical units might not match their physical measurements. Alternatively if the anchor unit is a physical unit, the pixel unit might not map to a whole number of device pixels.
In particular, in previous versions of CSS the pixel unit and the physical units were not related by a fixed ratio. Instead the physical units were always tied to their physical measurements while the pixel unit would vary to most closely match the reference pixel

the physical units were always tied to their physical measurements while the pixel unit would vary to most closely match the reference pixel. For reading at arm’s length, 1px thus corresponds to about 0.26 mm (1/96 inch)

-----

To be resolution independent, a system must able to scale content based on rendering conditions

unit which can then be mapped to a number of device pixels and scaled as desired e.g. when the user zooms the content.

on modern screens: 1in = 1in, relate the physical units to their physical measurements
anti-aliasing

on low screens: 1px = whole number of device pixels that best approximates the reference pixel

- device-width is in device pixels not CSS pixels !!!
usually physical units are first translated to CSS pixels, therefore vary with DPI !!!


## Position

#### Coordinate system
- position is always taken relative to the outside edges of a content- / padding- / border- or margin-box of an element
- axes: x-axis to right, y-axis to bottom, origin in top left corner
- keywords: top, right, bottom, left, center
- values: \[length], offset in x (or y) direction
- beware, that here % is relative to the element’s containing block which is not necessarily the element’s parent

#### Notation
- ::keyword::: horizontal or vertical direction, other defaults to center
- ::value::: horizontal direction, vertical defaults to 50%
- ::keyword keyword ::: each direction, order irrelevant
- ::value value::: horizontal direction then vertical, order relevant
- ::keyword value::: horizontal direction then vertical
- ::value keyword::: horizontal direction then vertical
- ::keyword keyword value::: each direction with second offset, order irrelevant
- ::keyword value keyword::: each direction with first offset, order irrelevant
- ::keyword value keyword value::: each direction with both offset, order irrelevant