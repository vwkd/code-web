---
title: Introduction
author: vwkd
index: 1
tags:
  - html
---

## Syntax

- whitespace is collapsed to single space, like in LaTeX, i.e. line breaks, indentation etc. just for readability
- file extension `.html`



## Document structure

- made up of elements, nested as family tree
- elements are used to add meaning to content
- `<!DOCTYPE>`: document type declaration
- `<html>`: entire content of web page
- `<head>`: meta data, e.g. scripts, styles, etc.
- `<body>`: visible content, e.g. text, images, etc.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>Welcome</h1>
    <p>Hello World!</p>
  </body>
</html>
```



## Elements

```html
<elementname attribute1="value1" attribute2="value2">...</elementname>
```

- (element, opening tag, attribute, content, closing tag)
- "empty" elements don't have content and closing tag, e.g. `<br>`, can optionally close opening tag itself, e.g. `<br/>`, only required in XML
- element names are case insensitive, convention is to use lower case
- unknown elements are ignored by browser, i.e. backwards compatible, but no feedback on mistake
- use elements according to semantic meaning, not default style, can always change style with CSS ❗️
- presentational elements are deprecated in favour of CSS, e.g. `<b>`
- beware: invalid markup is silently ignored, e.g. unsupported element, misspelled attribute, etc., no way to see errors ⚠️

### Important elements

| Element | name | notes |
| - | - | - |
| `<p>` | paragraph | |
| `<h1>`,&nbsp;...,&nbsp;`<h6>` | heading, big to small | |
| `<img>` | image | attributes: `src` source URL, `alt` alternate text, `width`/`height` width/height |
| `<a>` | anchor | attributes: `href` address, `target` window mode |
| `<br>` | line break | empty element |
| `<hr>` | horizontal rule | empty element |
| `<!--...-->` | comment | only element that is allowed before DOCTYPE |

- (hyper)link: `<a>`, `<area>`, or `<link>` element that has an `href` attribute



## Attributes

```html
attributename="attributevalue"
```

- name-value pairs
- specify additional information for element, control behavior of element
- always specified in opening tag
- multiple attributes are separated by space, e.g. `id="cnt1" class="red"`
- attribute names are case insensitive, convention is to use lower case
- attribute values are mostly case insensitive, with exceptions, e.g. of `id`, `class`
- quotes only required if attribute value contains space, but convention to always use
- browser ignores unknown attributes, i.e. backwards compatible, but no feedback on mistake
- if single value is same as name is called "boolean attribute", can use shorthand `attr` instead of `attr="val"`

```html
<div hidden>...</div>
<input type="checkbox" checked>
<input type="text" disabled>
<video src="..." controls>...</video>
```

### Global attributes

- work for almost all elements
- beware: prefer `class` over `id` since can add to more elements later ❗️
- `id` and `class` are mostly used as hooks for CSS and JavaScript, except `id` is also used for fragment identifier and `for` attribute of `<label>`

| attribute | description | example |
| - | - | - |
| `id` | document wide unique identifier, value must start with character, value is case sensitive | `id="cnt1"` |
| `class` | document wide non-unique identifier, value must start with character, multiple separated by space, value is case sensitive | `class="cnt"` |
| `lang` | language and dialect | `lang="en-US"` |
| `style` | inline CSS style sheet | `style="color:red;"`  |
| `title` | tooltip on mouseover | `title="NASA rocket"` |



## Character references

- string to escape special characters
- needed to refer to reserved characters, e.g. &lt;, &gt;, etc.
- can use to refer to common special characters as well, e.g. non-breaking space, &copy;, etc., but just write Unicode character instead to not hinder readability of source code

### Numeric character references

```html
&#D...D;
&#xH...H;
```

- whole Unicode coded charset
- D...D: Unicode code point in decimal
- H...H: Unicode code point in hexadecimal

| Numeric Character reference | Character           |
| --------------------------- | ------------------- |
| `&#8209;`                   | non-breaking hyphen |

### Character entity references

```html
&name;
```

- commonly used character references which have been given simple names
- name is case sensitive
- can remember more easily than Unicode code point

| Character entity reference | Character          |
| -------------------------- | ------------------ |
| `&quot;`                   | &quot;             |
| `&amp;`                    | &amp;              |
| `&apos;`                   | &apos;             |
| `&lt;`                     | &lt;               |
| `&gt;`                     | &gt;               |
| `&nbsp;`                   | non-breaking space |
| `&copy;`                   | &copy;             |
| `&reg;`                    | &reg;              |



## Compatibility

- (also helps SEO)
- specify meta data in `<head>`, e.g. language, title, charset
- structure page using HTML5 semantic elements, e.g. `<footer>`
- use semantic elements instead of meaningless, e.g. `<h1>` and not `<span>`
- use styling instead of semantically wrong elements, e.g. only `<h1>` because is bold
- use attributes, e.g. alternative text for images, title for links