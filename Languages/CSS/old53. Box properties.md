<!-- ToDo: incorporate, then delete -->

### Box properties


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