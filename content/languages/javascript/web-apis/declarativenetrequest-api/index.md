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
- declarative rules, JSON
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

### Selection
Matching precedence

1. matching condition
2. highest priority
3. action
  1. `"allow"`
  2. `"allowAllRequests"`
  3. `"block"`
  4. `"upgradeScheme"`
  5. `"redirect"`
  6. `"modifyHeaders"`

if multiple with same action and action has different properties, not well defined

between multiple extension, order is
1. "block"
2. "redirect", "upgradeScheme"
3. "allow", "allowAllRequests"



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

### Session

- don't persist across sessions or extension updates
managed using JavaScript while an extension is in use
- at most 5,000
- extension can add / remove using `updateSessionRules`



## Resources

- MDN - as usual
