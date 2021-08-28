---
title: Elements
author: vwkd
index: 2
tags:
  - languages
  - html
---

- use semantic elements as often as possible
- always think about accessibility for every part of web page, e.g. blind person, deaf person, etc.



## Meta elements

- used to specify metadata of document, e.g. title, structure, appearance, behavior, etc.
- `<html>`, `<head>`, `<body>` are optional in HTML5, if omitted are inferred from structure, but better include

| tag | description |
| - | - |
| `<html>` | root element of document |
| `<head>` | container for meta data of document |
| `<body>` | container for visible content of document |
| `<title>` | title, for browser tab, favorites, search engines |
| `<style>` | styles, by default assumes CSS |
| `<script>` | scripts, by default assumes JavaScript |
| `<link>` | links external resources to document, empty element |
| `<meta>` | specifies meta data of document, empty element, e.g. `charset` [^1], `refresh rate`, `width` for browsers, `author`, `description`, `keywords` [^2] |
| `<base>` | set the base URL for all relative URLs in doc |

[^1]: It's indeed paradoxical to declare the encoding of a file inside of itself. It's only a fallback if the HTTP header `Content-Type` is missing. Since most browsers by default read using an ASCII compatible encoding, one hopes that the file encoding is ASCII compatible and that there are only ASCII characters until this `meta` statement at the beginning of the file.
[^2]: `keywords` isn't used by search engines anymore because of abuse



## Structure elements

- used to group content
- can use CSS to style or layout these groups, or JS to add dynamic behavior
- use groups only for styling sparingly, instead use CSS inheritance ❗️
- use semantic elements if possible, e.g. `<header>` instead of `<div id="header">`, „divitis“ is overuse of `<div>` elements for everything ❗️

| tag | description |
| - | - |
| `<main>` | main content of the document, only once |
| `<header>` | header for the document or a section |
| `<footer>` | footer for the document or a section |
| `<nav>` | container for navigation links |
| `<section>` | related part of page, single functionality |
| `<article>` | independent self-contained content |
| `<aside>` | content aside from main content, e.g. sidebar |
| `<figure>` | dependent self-contained content with independent position, `<figcaption>` for caption, e.g. code block, illustration |
| `<details>` | additional details, can view or hide, `<summary>` for heading |
| `<mark>` | highlighted text |
| `<time>` | human-readable date/time |
| `<div>` | container element, non-semantic, avoid if possible |
| `<span>` | container element, non-semantic, avoid if possible |



## Formatting elements

- used to format content
- beware: use according to semantic meaning, not user agent default style, can always change style with CSS ❗️

| tag | semantic meaning | default style |
| - | - | - |
| `<strong>` | strong text | bold |
| `<em>` | emphasised text | italic |
| `<ins>` | inserted text | underline |
| `<del>` | deleted text | strike-through |
| `<mark>` | highlighted text | yellow background |
| `<small>` | smaller text | " |
| `<sub>` | subscripted text | " |
| `<sup>` | superscripted text | " |
| `<q>` | inline quotation | quotation marks |
| `<blockquote>` | block quotation, attributes: `<cite>` for source | block, indented margins |
| `<abbr>` | abbreviation | dotted underline |
| `<address>` | address | block, italic |
| `<cite>` | citation | italic |
| `<bdo>` | bidirectional text overwrite, text direction, attributes: `dir = "rtl", "ltr"` | |
| `<pre>` | preformatted text | block, monospace, preserves spaces and line breaks |



## Presentational elements

- used to style content
- beware: don't use, lack semantic meaning, use CSS instead ⚠️

| tag | description |
| - | - |
| `<b>` | bold text, like `<strong>` but without semantic meaning |
| `<i>` | italic text, like `<em>` but without semantic meaning |
| `<u>` | underlined text, like `<ins>` but without semantic meaning |
| `<s>` | strike-through text, like `<del>` but without semantic meaning |