---
title: Lists
author: vwkd
index: 3
tags:
  - languages
  - html
---

- `<ul>`, `<ol>` un/ordered (bullet/number) list
- `<li>` list item, can contain any type of element, i.e. can nest other list
- `<dl>`, `<dt>`, `<dd>` description list, -term, -description
- style with CSS property `list-style-type` instead of type-attribute

```html
<!-- basic list -->
<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ul>

<ol>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol>
```

```html
<!-- horizontal menu -->
<ul>
  <li><a href="#home">Home</a></li>
  <li><a href="#products">Products</a></li>
  <li><a href="#about">About</a></li>
  <li><a href="#contact">Contact</a></li>
</ul>
```

<!-- Demo: HTML/list -->



## Attributes

- `start`: set starting point for counting in `<ol>`