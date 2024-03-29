---
title: Strings
author: vwkd
index: 7
tags:
  - languages
  - javascript
---

- primitive values, constructor `String` wraps around to make them behave like objects, inherits some properties, see Data types as objects
- immutable array-like objects, can not change characters
- characters are indexed elements
- characters use UTF-16 encoding
- length property is number of UTF-16 code units, i.e. rare characters use two code units ("surrogate pairs")



## `String` object type

- `String.prototype.charAt()`: returns character at index, same as accessing object property using bracket notation, e.g. `str[5]`
- `String.prototype.charCodeAt()`: returns UTF-16 code unit at index as decimal integer, i.e. only first part if surrogate pair
- `String.prototype.charPointAt()`: returns UTF-16 code point at index as decimal integer, i.e. whole surrogate pair
- `String.prototype.includes()`: returns true if string contains provided string
- `String.prototype.endsWith()`: returns true if string ends with provided string
- `String.prototype.startsWith()`: returns true if string starts with provided string
- `String.prototype.indexOf()`: returns index of first match, `-1` if no match is found
- `String.prototype.lastIndexOf()`: returns index of last match, `-1` if no match is found
- `String.prototype.match()`: returns array with matches of RegEx search
- `String.prototype.matchAll()`: returns array with matches of RegEx search, including capturing groups
- `String.prototype.search()`: returns index of first RegEx match, `-1` if no match is found
- `String.prototype.replace()`: returns string with replacement of (RegEx) match
- `String.prototype.slice()`: returns section of string
- `String.prototype.split()`: returns array with splitted sections of string by separator
- `String.prototype.toUpper`/`LowerCase()`: returns string in upper- / lowercase
- `String.prototype.trim()`: returns string with whitespace trimmed from both ends
- `String.prototype.trimStart()`: returns string with whitespace trimmed from start, respects string locale (with ES2019)
- `String.prototype.trimEnd()`: returns string with whitespace trimmed from end, respects string locale (with ES2019)
- `String.prototype.padStart()`: returns string with other string padded at start until target length, default string is space, respects string locale (with ES2017)
- `String.prototype.padEnd()`: returns string with other string padded at end until target length, default string is space, respects string locale (with ES2017)



## Resources

- MDN - as usual