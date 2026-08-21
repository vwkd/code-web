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
? can't modify restricted browser requests and requests made by another extensions



## Rule

### Fields

#### ID

- identifier of rule
- positive integer
- mandatory
- must be unique within ruleset

#### Priority

- priority of rule
- positive integer
- defaults to `1`

#### Condition

- condition when rule matches request

##### `urlFilter`

- pattern matching the network request URL
- can't use with `regexFilter`
- defaults to all
- (optional) left or domain name anchor + pattern + (optional) right anchor
- `*` : wildcard, matches any number of characters
- `|` : left/right anchor, at beginning/end specifies beginning/end of URL
- `||` : domain name anchor, at beginning specifies start of a (sub-)domain of the URL
- `^` : separator character, matches any single character except letter, digit, `_`, `-`, `.`, or `%.`, at end also matches end of URL
- can't use `||*` at beginning, instead use `*`
- must be ASCII
- note: use URL-encoding for special characters
- note: use Punycode for internationalized domains

##### `regexFilter`

- regular expression matching the network request URL
- can't use with `urlFilter`
- must be ASCII
- note: use URL-encoding for special characters
- note: use Punycode for internationalized domains

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

##### `isUrlFilterCaseSensitive`

- whether `urlFilter` / `regexFilter` is case sensitive
- defaults to `false`

##### `requestDomains`

- domains of request
- defaults to any
- must be lowercase ASCII
- use Punycode for internationalized domains
- note: includes subdomains

##### `excludedRequestDomains`

- like `requestDomains`, but reverse
- takes precedence over `requestDomains`

##### `resourceTypes`

- resource type of request
- `main_frame`: top-level document, loaded in tab
- `sub_frame`: document, loaded in iframe or frame
- ...
- if `excludedResourceTypes` isn't specified, defaults to anything except `main_frame`
- if action is `allowAllRequests`, mandatory and can only include `main_frame` and `sub_frame`

##### `excludedResourceTypes`

- like `resourceTypes`, but reverse
- takes precedence over `resourceTypes`

##### `initiatorDomains`

- domains from which request originates
- defaults to any
- must be lowercase ASCII
- use Punycode for internationalized domains
- note: includes subdomains

##### `excludedInitiatorDomains`

- like `initiatorDomains`, but reverse
- takes precedence over `initiatorDomains`

#### Action

- action of rule

##### `type`

- type of action

###### `allow`

- ignore other matching block/redirect rules

###### `allowAllRequests`

- like `allow`, but also any other requests in frame hierarchy
also applies to future subresource loads in the document (including descendant frames) generated from the request

for main_frame and sub_frame resourceTypes only

###### `block`

- block request

###### `upgradeScheme`

- upgrade schema of request
- i.e. http, ftp to https

###### `redirect`

- redirect request

? redirected request is evaluated again
  source matching and destination/initiator exclusions must prevent loops

###### `modifyHeaders`

- change request and/or response headers

##### `redirect`

- target of redirect
- only if `redirect` action
- not applied if
  - doesn't change request ?? MEANS SAME URL
  - invalid redirect URL
- `url`: redirect URL, no JavaScript URL
- `transform`: redirect URL component transformations
- `regexSubstitution`: substitution pattern for first match of `regexFilter`

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

- packaged in extension manifest file, at most 100
  - in `"declarative_net_request.rule_resources"` manifest key
  - in JSON format
- extension can enable / disable using `updateEnabledRulesets`, at most 50 at a time
- silently ignored if invalid
- beware: make sure are valid!
- persist across sessions, not extension updates
- at most 30,000

### Dynamic

- managed using JavaScript while an extension is in use
- extension can add / remove using `updateDynamicRules`
- beware: one invalid rule aborts entire update!
- persist across sessions and extension updates
- at most 30,000, of those at most 5,000 unsafe

### Session

- managed using JavaScript while an extension is in use
- extension can add / remove using `updateSessionRules`
- beware: one invalid rule aborts entire update!
- don't persist across sessions or extension updates
- at most 5,000



## Resources

- MDN - as usual
