---
title: Introduction
author: vwkd
index: 1
tags:
  - languages
  - graphql
---

## Document

- string that contains definitions

```plaintext
Document:
   ExecutableDefinition+
   TypeSystemDefinition TypeSystemExtension*

ExecutableDefinition:
    OperationDefinition
    FragmentDefinition
```

- beware: differs from spec, spec allows for partial documents and mixes of query language and IDL, here assume document contains either executable operations or single schema, see [#788](https://github.com/graphql/graphql-spec/issues/788#issuecomment-721298885) ⚠️

spec has multiple documents, i.e. grammar has multiple root nodes, doesn't make sense, should just have one root node and say can implement only some of the options, currently this is implcit outside the spec in the word "GraphQL" which might refer to any one of those 4 without further clarification



### Grammar

- white space, line terminators, commas are not significant, only stylistic, e.g. argument list, array, etc.
- comments are lines starting with `#`
- beware: comments are different than in JS, not `//` ❗️
- names can consist of `[a-zA-Z0-9_]` but not start with a digit
- names are case sensitive
- lists are unordered, e.g. argument list, selection list, etc.



## Request / Response

- server runs GraphQL engine that can execute operations
- client builds string of document containing executable operation(s), sends request to server along with variables and name of operations to run (if document contains more than one operation)
- server verifies operation against schema, runs resolver functions, returns response with data as JSON
??? - by convention data is under `data` key in JSON object, `error` key for errors, ... ?



## Errors

<!--
??? if query errors gets partial response by default, needs to do error handling on client

??? error list
field is set to `null` value + error is added to list
error never just makes field `null` without other information
if error and field type is non-null, then field error is thrown ?? parent field is set to null ??

allows a nullable field to return null if it fails, e.g. server down, typo in name, etc.
if error on non-nullable field, bubbles up until nearest nullable parent type and nulls that

doesn't use HTTP verbs!!!, e.g. if query failed still 200 OK (except if server itself has problem) 
-->



## Resources

- [GraphQL - Specification 11/2020](http://spec.graphql.org/draft/)
- [GraphQL Playground](https://graphql.org/swapi-graphql/)
- [Lee Byron - Lessons From 4 Years of GraphQL](https://www.graphql.com/articles/4-years-of-graphql-lee-byron)