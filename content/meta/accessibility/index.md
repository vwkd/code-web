---
title: Accessibility
author: vwkd
index: 5
tags:
  - meta
---

<!-- ToDo: Finish -->

??



## HTML

- use semantic markup
- use ARIA roles for unsemantic markup, e.g. search field
- alt tags in content media, e.g. images, video, audio, etc.
- empty alt tags in decorative media content, e.g. images, etc.



## CSS

- color contrast ratio
- use not only color to distinguish function, e.g. text, icon, etc. 
- preference media queries, e.g. dark mode, reduced motion, color contrast, etc.
- use `:focus(-within)` along `:hover` for showing elements, e.g. dropdown, tooltip, etc.



## JavaScript

- in SPA after client-side routing put focus on top of page, read title



## Browser support

- define data driven cut-off point, when income due to these customers doesn't warant cost of support anymore
- define cut-off point relative instead of absolute, e.g. at most last N versions of a non-dead browser with more than X% market share
- show update warning below cut-off point, don't break silently, e.g. fullscreen message
- browser detection based on HTTP User Agent Hint headers, deliver minimal page with update warning instead of whole page
- beware: as of 2020 HTTP User Agent Hints are still only a proposal, see [Improving user privacy and developer experience with User-Agent Client Hints](https://web.dev/user-agent-client-hints/)



## Resources