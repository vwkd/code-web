### Box properties

#### Size
- `width`, `height`: size of content box
- `min-`/`max-width`, `height`: min- / maximal size of content box
- `auto` (default, adjust to content), \[length], % (of element’s containing block)
- text content respects width but not height, can be controlled with `overflow` property
- children can overwrite parent size with separate width
- for full size of element needs to sum up padding, border and margin, i.e. for `width:100%` and any padding, border and margin it will overflow out of it’s parent
- the behavior can be changed with `box-sizing` property

#### Padding
- `padding`: \[length] (default, 0), % (of _width_ of element’s containing block)
- for each of the above can also insert either „top“, „right“, „bottom“, or „left“ to set only one individual side

#### Border
- `border-style` (required): style of all four borders
	`none` (default), `dotted`, `dashed`, `solid`, `double`, `groove`, `ridge`, `inset`, `outset`, `hidden` 
- `border-color`: color of all four borders
	\[color] (default, black), `transparent`
- `border-width`: width of all four borders
	`medium` (default), `thin`, `thick`, \[length]
- `border`: shorthand for all above, order is free
- `border-radius`: border-radius for all four corners
	\[length] (default, 0)
- for each of the above can give also 2, 3 or 4 values for individual sides
- for each of the above can also insert either „top“, „right“, „bottom“, or „left“ to set only one individual side
- to set 3 sides the same and 1 different, set for all 4, then overwrite for 1

#### Margin
- `margin`: \[length] (default, 0), % (of _width_ of element’s containing block), `auto` (take up remaining space)
- negative values are allowed
- for each of the above can also insert either „top“, „right“, „bottom“, or „left“ to set only one individual side

- for adjacent elements horizontal margins add, vertical margins collapse to only the larger margin of both margins (except for floating and absolutely positioned elements)
- If left and right margins are set to `auto`, the remaining space will be equally divided between the two i.e. element will be centered (only works with fixed width block element that is not absolutely or fixed positioned)

#### Outline
- border outside of element border, not part of element, may overlap other content, doesn’t affect size or position
- `outline-style` (required): like borders
- `outline-color`: like borders
- `outline-width`: like borders
- `outline`: like borders
- `outline-offset`: transparent padding between element border and outline
	\[length] (default, 0)

#### Background
- `background-color`: color, always set in case image is not available
	`transparent` (default), \[color]
- `background-image`: one or more images
	`none` (default), `URL('…')`, `linear-gradient()`, `radial-gradient()`
- `background-repeat`: repeat image to fill container
	`repeat` (default), `repeat-x` / `-y`, `no-repeat`, `space`, `round`
- `background-position`: position of image within parent
	\[position] (default, top left; using percent might not behave as expected)
- `background-size`: size of image
	`auto` (default, original size), `cover` (scale to fill all, no stretching), `contain` (scale to fill, no cropping or stretching), \[length], % (of positioning area) 
- `background-origin`: outer edge where the image has its origin
	`border-box` (default), `padding-box`, `content-box`
- `background-clip`: outer edge until which the image extends
	`border-box` (default), `padding-box`, `content-box`

- `background-attachment`: background fixed or scroll
	`scroll` (default, scroll with page), `fixed` (fix to viewport), `local` (scroll with content)
- `background`: shorthand for all above, order is free

#### Overflow
- `overflow`: control behavior if content doesn’t fit in element’s box
- only applies to block elements with a specified size
- `visible` (default, not clipped), `hidden` (clipped and hidden), `scroll` (clipped, scroll bars), `auto` (clipped, scroll bars when needed)
- `overflow-x` / `-y`: the above just in horizontal / vertical direction