---
title: Declarative Net Request (DNR) API
author: vwkd
index: 12
tags:
  - languages
  - javascript
  - web-apis
---

- API to modify network requests
- e.g. block, redirect, etc.
- declarative rules
- extension doesn't need to intercept requests



## Rule

Block a network request.
Upgrade the schema (http to https).
Prevent a request from getting blocked by negating any matching blocked rules.
Redirect a network request.
Modify request or response headers.

### Fields

#### ID

unique identifier of rule within ruleset
mandatory
>= 1

#### Priority

rule priority
>= 1
defaults to 1

positive integer
defaults to 1 when not set

#### Condition

condition under which rule is triggered

##### `urlFilter`

##### `regexFilter`

###### Safari

- uses WebKit's restricted Content Extensions regex grammar, not Chrome's RE2 grammar
- only supports
  - ASCII literals, escaped literals
  - character classes, e.g. `[a-z0-9]`, `[^/?#]`
  - non-/capturing groups, e.g. `(...)`, `(?:...)`
  - `.`, `?`, `*`, `+`
  - anchors `^`/`$` as first/last character, e.g. not in group
- doesn't support anything else, e.g.
  - non-ASCII literals
  - alternation, e.g. `|`
  - general counted repetitions, e.g. `{2}`, `{2, 5}`
  - shorthand character classes, e.g. `\d`, `\w`, `\s`, `\b`
  - lookaround, e.g. `(?=...)`, `(?!...)`, `(?<=...)`, `(?<!...)`
  - anchors `^`/`$` not as first/last character, e.g. in group
  - inline modifiers, e.g. `(?i)`
  - named backreferences
  - etc.
- see https://github.com/w3c/webextensions/issues/344

#### Action

action to take when the rule is matched

block a network request
redirect a network request
modify headers from a network request
prevent another matching rule from being applied

##### `allow`

allow the request
used ignore other matching rules

##### `allowAllRequests`

for main_frame and sub_frame resourceTypes only
same as `"allow"` but also applies to future subresource loads in the document (including descendant frames) generated from the request

##### `block`

cancels the request

##### `upgradeScheme`

upgrades the scheme of the request

##### `redirect`

redirects the request

has no effect if
  - action does not change the request ?? MEANS SAME URL
  - invalid redirect URL, e.g. `regexSubstitution`

##### `modifyHeaders`

rewrites request and/or response headers

### Evaluation

#### Before request

- condition without `responseHeaders`
- applies groups in order

##### Group 1

- action `allow`, `allowAllRequests`, `block`, `upgradeScheme`, `redirect`
- applies one matching rule
- for matching condition, precedence

1. priority
2. action
  1. `allow`, `allowAllRequests`, request's frame previously matched an `allowAllRequests` rule of same or higher priority
  2. `block`
  3. `upgradeScheme`
  4. `redirect`

- if still multiple, undefined, i.e. same action with different properties
- for multiple extensions, precedence

1. action
  1. `block`
  2. `redirect,` `upgradeScheme`
  3. `allow,` `allowAllRequests`

- if still multiple, from most recently installed extension

##### Group 2

- action `modifyHeaders`
- if higher priority than any matching rule in group 1 with `allow` or `allowAllRequests` action
- applies all matching rules
- for matching condition and same header, precedence

1. priority

- if higher priority rule sets or appends to header, applies lower priority rule that appends to it
- if higher priority rule removes header, doesn't apply any lower priority rules
- between multiple extensions, from most recently installed extension

##### After response

- condition with `responseHeaders`
- like before request



## Ruleset

- collection of rules
- limited due to performance
- safe if action is `allow`, `allowAllRequests`, `block`, `upgradeScheme`
- ? i.e. unsafe if action is `redirect`, `modifyHeaders`
- for each rule type at most 1,000 regex rules

### Static

- persist across sessions, not extension updates
Packaged, installed, and updated when an extension is installed or upgraded
stored in rule files in JSON format, listed in manifest file
- at most 30,000
- silently ignored if invalid
- beware: make sure are valid!
- included in extension manifest file using `"declarative_net_request.rule_resources"` manifest key, at most 100
- extension can enable / disable using `updateEnabledRulesets`, at most 50 at a time

### Dynamic

- persist across sessions and extension updates
managed using JavaScript while an extension is in use
- at most 30,000, of those at most 5,000 unsafe
- extension can add / remove using `updateDynamicRules`
- beware: one invalid rule aborts the complete update!

### Session

- don't persist across sessions or extension updates
managed using JavaScript while an extension is in use
- at most 5,000
- extension can add / remove using `updateSessionRules`
- beware: one invalid rule aborts the complete update!



## Resources

- MDN - as usual
