---
title: Introduction
author: vwkd
index: 1
tags:
  - css
---
# Introduction



## CSS (Cascading Style Sheets)

- standard style sheet language of markup documents, e.g. HTML
- describes _presentation_ of Web page and _appearance_ of content
- for _structure_ see HTML, for _behavior_ see JavaScript, e.g. text could be bold to emphasise it (structure) or to make it look better (appearance)
- file extension `.css`
- whitespace is collapsed to single space, like in LaTeX, i.e. line breaks, indentation etc. just for readability
- multi-line comments `/*...*/`, no single-line comments



## Document structure

- made up of rule-sets, sequenced linearly

```css
h1 {
  color: red;
  text-align: center;
}

p {
  color: blue;
  font-size: 12px;
}
```

- rule(-set): `h1 { ... }`, `p { ... }`, etc
- declaration block: `{ ... }`
- selector: `h1`, `p`, etc.
- declaration: `color: red`, `font-size: 12px`, etc.
- property: `color`, `font-size`, etc.
- value: `red`, `12px`, etc.



## Types

### External 👍

- style in separate `.css` file
- reference `.css` in `<link>` element anywhere in `<head>` section

```html
<link rel="stylesheet" href="...css">
```

- can omit `type` attribute since CSS is default style sheet language
- best practice, separates CSS from HTML
- separates presentation from content, reduces complexity and repetition, offers more flexibility and control

### Internal 👎

- style in `.html` file
- style in `<style>` element anywhere in `<head>` section

```html
<style>...</style>
```

- not recommended, mixes CSS with HTML
- needs to duplicate style for different page

### Inline 👎

- style in `.html` file
- declarations in `style` attribute of single HTML element
- no selector is needed since element is known

```html
<tagname style="property1: value1; property2: value2;">...</tagname>
```

- not recommended, mixes CSS with HTML
- needs to duplicate style for different elements



## Rule-set

```css
selector1, selector2 {
  property1: value1;
  property2: value2;
}
```

- specifies the style of the selected element(s)
- semicolon between declarations, last semicolon optional but makes later extension easier
- beware: invalid styles are silently ignored, e.g. unsupported properties, misspelled value, etc., no way to see errors ⚠️



## Selectors

<!-- todo: factor out into own file, when adds newer selectors like is(), not(), where(), etc. -->

```css
selector1 {
  /* ... */
}
```

- selects the element(s) a style is applied to
- like search operator, selects all elements that match the query
- can select by type, relationship, attributes, states, etc.
- mostly case insensitive, except for some attributes, e.g. `id`, `class`
- can compose selectors for more specific selections using no whitespace, type selectors must come first, e.g. `a#myid:hover`, `a[href*="pic"]`, etc.
- can group multiple selectors for identical style using comma, e.g. `selector1, selector2 { ... }`
- don't use only structure to set style, hard to maintain, can't reuse rules, e.g. `main > ul > li > a { ... }`, etc.
- don't use only classes to set style, "classitis", hard to maintain, needs to add classes to every element in HTML, e.g. `<p class="green large bold serif">...</p>`, etc.
- needs to find middleground between structure and classes
- beware: can set everything in one selection and unset exception in separate selection, often easier than excluding exception from selection ❗️

```css
/* simple */
myel {
  color: red;
}

myel:last-of-type {
  color: initial;
}
```

```css
/* complex */
myel:not(last-of-type) {
  color: red;
}
```

### Basic selectors

| selector | description |
| - | - |
| `myelement` | elements of this _element type_ |
| `*` | elements of any element type, _all elements_ |
| `#myid` | single unique element with this _ID-attribute_ |
| `.myclass` | elements with this _class-attribute_ |

- recommended to use lowercase hyphenation for own identifiers, e.g. class, ID, etc.

### Combinator selectors

| selector | description |
| - | - |
| `myel1 myel2` | elements of type `myel2` that are _descendants_ of elements with type `myel1`, not only immediate children |
| `myel1 > myel2` | elements of type `myel2` that are _children_ of elements with type `myel1`, only immediate descendants |
| `myel1 ~ myel2` | elements of type `myel2` that are _general siblings_ of elements with type `myel1`, i.e. all elements after `myel1` in same tree depth |
| `myel1 + myel2` | elements of type `myel2` that are _adjacent siblings_ of elements with type `myel1`, i.e. single elements directly after `myel1` in same tree depth |

### Attribute selectors

| selector | description |
| - | - |
| `[attr]` | elements with this _attribute_ |
| `[attr="val"]` | elements with this _attribute and value_ |
| `[attr*="val"]` | elements with this attribute _containing_ this value |
| `[attr~="val"]` | elements with this attribute _containing_ this value surrounded only by spaces |
| `[attr^="val"]` | elements with this attribute _starting_ with this value |
| `[attr$="val"]` | elements with this attribute _ending_ with this value |
| `[attr|="val"]` | elements with this attribute _starting_ with this value followed only by a hypen |

- attribute value by default case-sensitive in HTML, can use `i` modifier to match case-insensitively
- except value for `type` attribute by default case-insensitive in HTML, can use `s` modifier to match case-sensitively

### Pseudo-class selectors

- pseudo-class: certain state of elements, state can be thought of as class of elements
- state may be limited to certain elements, e.g. links, input elements, etc.

| selector | description | note |
| - | - | - |
| `:hover` | elements _moused-over_ | beware: not well defined for touch input ❗️|
| `:focus` | elements _in focus_, e.g. using tab | for all input devices in all situations, e.g. keyboard and mouse |
| `:focus-visible` | elements _in focus_, e.g. using tab | for certain input devices in certain situations determined by UA, e.g. keyboard but not mouse |
| `:active` | elements _selected_, e.g. using click, enter | `:active` implies `:focus` |
| `:first-child` | elements that are _first children_ of their parents | |
| `:first-of-type` | elements that are _first of their type_ inside their parents | |
| `:nth-child(n)` | elements that are _nth children_ of their parents | |
| `:nth-of-type(n)` | elements that are _nth of their type_ inside their parents | |
| `:only-child` | elements that are _only child_ of their parents | |
| `:only-of-type` | elements that are _only of their type_ inside their parents | |
| `:any-link` | hyperlinks | see [HTML/Introduction](../HTML/1.%20Introduction.md#Important_elements) for defintion of hyperlink |
| `:link` | hyperlinks _not yet visited_, i.e. not in user agent history | see [HTML/Introduction](../HTML/1.%20Introduction.md#Important_elements) for defintion of hyperlink |
| `:visited` | hyperlinks _already visited_, i.e. in user agent history | see [HTML/Introduction](../HTML/1.%20Introduction.md#Important_elements) for defintion of hyperlink |
| ... | ... |

- beware: `:link`, `:visited`, `:hover`, `:focus`, `:active` overwrite each other, must be in this order to work as expected ❗️
- beware: for privacy a user agent may lie about pseudo-class or limit properties that can be set, e.g. in `:visited` ❗️
- beware: `:hover` selects only box at top of stacking context and the establishing box, see Stacking ❗️
- can use `a[href^="https://"], a[href^="http://"], a[href^="//"]` to target external hyperlinks
- beware: don't use `a[href*="//"]` because would also match URLs with empty path segments, e.g. `a/valid//path` ❗️
- can use `:focus:not(:focus-visible)` to target inverse of beneficial input devices, e.g. mouse but not keyboard

### Pseudo-element selectors

- pseudo-element: certain part of an element, part can be thought of as element of its own

| selector | description |
| - | - |
| `::first-line` | _first line_ of text in element |
| `::first-letter` | _first letter_ of text in element |
| `::before` | _before_ the content of element, still within content |
| `::after` | _after_ the content of element, still within content |
| `::selection` | _selected part_ of an element |
| `::marker` | _marker box_ of an `display: list-item` element |
| ... | ... |

- `::before` and `::after` are usually used with the `content` property, see Generated content
- (pre CSS2 syntax used single colon like for pseudo-class, still backwards compatible)
- beware: pseudo-elements are not matched by any other selector including the universal selector ❗️



## Properties and values

```css
property1: value1;
```

- set a stylistic feature of the selected element(s)
- a property can affect a specific set of possible elements
- a property has a specific set of possible values
- a property might take only effect if element has certain style, e.g. `width` not if `display: inline`
- property names are hyphenated, case-insensitive
- CSS-wide values are accepted by all properties, "global values", e.g. `inherit`, `initial`, `unset`, etc.
- values have a data type, e.g. string, number, color, etc., see Data Types
- a property may accept its value in multiple data types, e.g. global values, percentage instead of number, etc.
- beware: don't confuse property that accepts its value in multiple data types with a property that accepts multiple values ❗️

### Shorthand properties

- combines multiple more specific properties
- accepts multiple values separated by space
- value data types and initial value are same as for individual properties
- order of values doesn't matter if types of values are different, e.g. `font`, `background`, `border-top`, etc.
- order of values with identical types for properties of box edges follow rule, e.g. `padding` combines `padding-top`, `padding-right`, `padding-bottom`, `padding-left`, or `margin`, `border-width`, `border-color`, `border-style`, etc.
  - 4: top, right, bottom, left, i.e. clockwise
  - 3: top, right & left, bottom
  - 2: top & bottom, right & left
  - 1: all edges
- order of values with identical types for properties of box corners follow rule, e.g. `border-radius` combines `border-top-left-radius`, `border-top-right-radius`, `border-bottom-right-radius`, `border-bottom-left-radius`
  - 4: top-left, top-right, bottom-right, bottom-left, i.e. clockwise
  - 3: top-left, top-right & bottom-left, bottom-right
  - 2: top-left & bottom-right, top-right & bottom-left
  - 1: all corners
- can set 3 properties by using shorthand to set all 4 and longhand to reset 1
- beware: `inherit` can be applied only to shorthand as a whole, use longhand to apply to individual values ❗️
- meta shorthands combine multiple shorthands, e.g. `border` combines `border-width`, `border-color`, `border-style`
- beware: omitted values are set to some values, usually but not necessarily their initial values, i.e. shorthand deletes previous values, prevents inheritance ⚠️

```css
border-top-width: 42px;
/* resets border-top-width to medium */
border-top: solid black;
```

```css
list-style-position: inside;
/* resets list-style-position to outside */
list-style: square;
```

- universal shorthand property `all` for every property, can use to reset everything

### Functions

- produce a value
- can be used as value if produce correct data type, e.g. `url()` as `<url>`, `hsl()` as `<color>`, etc.
- may produce different data types depending on input, e.g. `toggle()` produces same data type as input



## Defaulting

- elements have no default style, are all treated the same, i.e. CSS doesn't know difference between `<p>`, `<em>`, `<ul>`, `<table>`, etc., there are no properties that only apply to certain elements, but only properties that don't apply when already other properties are set, e.g. no size properties if `display: inline` ❗️
- any different appearances are set by user agent style sheet
- beware: always choose elements by semantic meaning, then style as desired, overwrite user agent default style, e.g. `<ul>` for navigation menu since it's a list ❗️
- beware: implementations are full of inconsistencies, user agents may still treat elements differently, e.g. form elements don't inherit font styling, etc. ⚠️

### Initial values

<!-- ToDo: set comment "beware: user agent style sheet may set any value, overrides initial value ❗️" on all properties or delete it and make it clear here, but not only one some like currently -->

- a property has an initial value, defined by specification, i.e. no property remains "undefined"
- the initial value for a property is used for _all elements_ for which no value is set and no one is inherited, see Inheritance below
- beware: initial value of a property is same for every element to which property can apply, part of property definition, not specific to certain element ⚠️
- a user agent may choose to apply a style sheet by default, usually styled for classical documents, e.g. `font-family` is serif, `<h1>` is big and bold, `<ul>` has indented bullet points, `<a>` with `href` attribute is blue and underlined, etc.
- beware: don't confuse initial value with value set by user agent style sheet, appears as if was an initial value, but user agent sets values normally like any other style sheet, initial value therefore is not used ⚠️
- beware: "default values" applied by user agent style sheet can apply only to some elements or only to certain state of one element, e.g. `<body>` has margin but not all other elements, or `<a>` with `href` attribute is blue but otherwise not, etc., therefore are not initial values ⚠️
- can undo user agent default style using `* {all: reset}`, resets each property to initial or inherited value
- beware: needs to style `:focus` manually for accessibility, e.g. outline, etc. ❗️
- can use a "reset" style sheet to undo spacing-related user agent default style, e.g. [destyle.css](https://github.com/nicolas-cusan/destyle.css)
- can use a "normalize" style sheet to unify user agent default style across different user agents, e.g. [normalize.css](https://github.com/necolas/normalize.css), [sanitize.css](https://github.com/csstools/sanitize.css), [cssremedy](https://github.com/jensimmons/cssremedy), [postcss-normalize](https://github.com/csstools/postcss-normalize), etc., see [html5-kitchen-sink](https://github.com/dbox/html5-kitchen-sink) for comparison

### Inheritance

- a property can be an inherited property, defined by specification, e.g. most text styling properties are inherited, like `color`, `font-size`, etc.
- a inherited property of a descendant element inherits the value from the property of its parent element
- the inherited value for an inherited property is used for _all elements_ for which no value is set instead of the initial value, only the root element uses the initial value because it has no parent
- beware: a inherited property inherits for every element to which property can apply, part of property definition, not limited to certain element ⚠️
- beware: an inherited property set by user agent style sheet makes it appear as if didn't inherit for that specific element, but property does inherit for every element, just user agent set a value for this element(s), e.g. `color` of anchor element (with `href` attribute) is blue ⚠️
- use inheritance to style majority of elements, style only exceptions separately, write same code only once, saves lot of work, e.g. not setting color for every individual element in tree

### Explicit defaulting

<!-- ToDo: Finish -->

- global values, can be set for any property
- specify defaulting behavior
- behave like any other value in cascade, see below

- `inherit`: inherited value, i.e. forces inheritance
- `initial`: initial value, beware: not "default value" from user agent style sheet, i.e. as if all declarations of the property for _all_ elements in style sheets of _any_ origin were erased ❗️
- `unset`: like `inherit` if property inherits, otherwise like `initial`, i.e. as if all declarations of the property for _this_ element(s) in style sheets of _any_ origin were erased
- `revert`: like `unset`, except limited to style sheet of current origin, earlier style sheets can still apply (see Priority below), i.e. as if all declarations of the property for _this_ element(s) in style sheets of _this_ origin were erased

- beware: don't confuse initial value with value set by user agent style sheet, even though initial value may be defined as being set by user agent, e.g. `color`, see [StackOverflow](https://stackoverflow.com/a/61869830/2607891) ❗️

```html
<p>Lorem ipsum <a href="#">dolor</> sit amet.</p>
```

```css
/* color is an inherited property and has initial value black (*) */

p {
  color: green;
}

a {
  /* blue, since user agent style sheet default */

  /* green, since inherits from parent p */
  color: inherit;

  /* black (*), since initial value */
  color: initial;

  /* green, since color is inherited property */
  color: unset;

  /* blue, since user agent style sheet default */
  color: revert;
}
```

- can use global values to make exception from a rule, e.g.

```css
/* make all paragraphs green */

p {
  color: green;
}

/* except in the sidebar */

#sidebar p {
  color: inherit;
}
```



## Cascade

- algorithm to determine style of an element
- rules can come from different style sheets or selectors
- all rules for the element are applied
- but for one property of the element there might be multiple declarations
- conflicting declarations for one property are prioritized according to the cascade

1. Sort declarations by origin and importance, apply only most specific
    - origins of stylesheet: user agent, user, author
    - importance of declaration: `!important` after property value
    - order: user agent < user < author < author `!important` < user `!important` < user agent `!important`
    - author order: ex-/internal < inline
    - if declarations have same origin and importance, go to 3.
2. Sort declarations by selector specificity, apply only most specific
    - specificity: weight of a compound selector, determined by types of constituent selectors
    - order: (pseudo-)element < (pseudo-)class, attribute < ID
    - order of constituent selectors doesn't matter, just quantity of each
    - universal selector, combinators, negation pseudo-class have no specificity
    - if declarations have same selector specificity, go to 4.
3. Sort declarations by rule appearance, apply only last defined

- order of rules and declarations doesn't matter, except when origin, importance and specificity are identical so that 3. is reached
- user stylesheet with `!important` wins over author `!important` because accessibility requires custom style 
- selector order doesn't matter for specificity, just quantity of selectors, e.g.  
  `body p` ≙ `main p`, `.myclass#myid` ≙ `#myid.myclass`
- can artificially increase selector specificity by chaining same ID or class, e.g.  
  `p.myclass` < `p.myclass.myclass` < `p#myid` < `p#myid.myclass` < `p#myid#myid`  
- use least specific selector, so can overwrite easily later
- use least amount of selectors, so can overwrite easily later and reuse rule for multiple elements
- don't use `!important`, behaves as if from style sheet of higher origin, not affected by selector specificity or order of appearance, can't be overwritten by non-important declarations



## Replaced elements

- element whose content is not affected by style sheet of document, e.g. `<img>`, `<iframe>`, `<video>`, `<embed>`, etc.
- style sheet can affect only element itself, e.g. position, size, layout, etc.
- uses intrinsic size based on contents by default, e.g. width, height, aspect ratio, etc.
- beware: needs to set adaptive size, otherwise may overflow due to fixed size ❗️



## At-rules

<!-- ToDo: Finish -->

- ?? 
- @import imports a stylesheet into another CSS stylesheet
???- beware: unlike JS modules not optimized for loading as fast as possible, executes, blocks until import is loaded, then continues ❗️
don't use, instead use multiple `<link>` attributes, e.g. with a media attribute to apply style based on device via media query


Media queries use conditional logic for applying CSS styling.



## Resources

- MDN