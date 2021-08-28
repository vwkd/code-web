---
title: Regular expressions (Regex)
author: vwkd
index: 8
tags:
  - javascript
---

- search pattern
- matches characters in a string

<!-- todo: incorporate

uses Regex object, has own methods exec() and test() and can be put into methods of String object match(), replace() etc.

Regex literal: /regexpression/
constructor function of Regex: new RegExp('regexpression')
   since uses string, needs to escape special characters with backslash, i.e. for backslash itself needs \\ and for one normal text-like backslash needs \\\\ (2 for string, 1 for regex, so one is escaped)

by default matches only one character, needs to use ???multiple character operators??? like *,+,{}
   by default is greedy and matches as many possible, needs to use ? to match as few as possible
by default doesn’t care what’s before or behind, can only match occurrence itself, needs to use lookahead or lookbehind to make it conditional
needs to escape special characters with backslash, but not inside character classs []


can add escape backslash to string automatically using
function escapeRegExp(string) {
  return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); // $& means the whole matched string
}

-->

<!-- ToDo: Finish -->

with ES2018:
- lookbehinds
- named capture groups
- s flag, dot all mode



## lookaheads / lookbehinds

- match only if condition before / behind ??
- e.g. start and end anchors