# Tutorials Style Guide

[TOC]

<!-- ToDo: Enforce Style Guide on existing documents -->

## Content

- begin with a 'Introduction' section to motivate the topic, introduce required terminology, etc.
- end with a 'Resources' section that lists most important sources for previous content and additional resources to learn more
- use bullet point writing style, each chunk of information explains only one thing, chunks sorted in logical order from simple to complex, like a mathematical proof
- don't duplicate existing chunks, instead refer to original chunk, except if critical warning that needs to remind about again
- feel free to rewrite and restructure sections, don't build "on top of it", shouldn't be a Wikipedia-like loose pile of facts, instead a coherent learning guide
- keep example code snippets as simple as possible, focus only on single purpose, not showcase of full use in production



## Style

- set 1 tab to 2 spaces, never use the tab character
- lists don't contain blank lines between items
- two break a new line within a list item, leave 2 spaces at end of list item and indent next line by 2 spaces
- put code boxes in top-level between two lists instead of in list item, only if code is in the middle of a second-level list don't outdent so second-level list doesn't break
- use sections with `##`, only first title is `#`
- use 3 blank lines between sections, only 1 blank line to separate any deeper hierarchy
- put 1 space between words, including unicode characters, e.g. `Hello World ❗️`



```markdown
# Title

[TOC]



## Introduction

- Lorem ipsum dolor sit amet
- consectetur adipiscing elit
- sed do eiusmod tempor incididunt



## Veni

- ut labore et dolore
  - magna aliqua
  - ut enim
- ad minim veniam

### Vidi

- quis nostrud exercitation

### Vici

- ullamco laboris nisi ut



## Quo vadis

- aliquip ex ea commodo consequat



## Resources

- [Lorem ipsum - dolor sit amet](#) and subpages
- [Ut enim - ad minim veniam](#)
```