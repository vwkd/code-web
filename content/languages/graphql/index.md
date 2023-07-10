---
title: GraphQL
author: vwkd
index: 6
tags:
  - languages
  - graphql
---

- API query language and IDL
- API query language is designed for JSON responses
- IDL makes documentation first-class, e.g. via introspection

//// nordicapis

predetermined, knowable format

application layer query language

interprets strings from the client, and returns data in an understandable, predictable, pre-defined manner
-> client specifiers what wants, gets what specifies

client defines the expected data format

single endpoint
variable request call
eliminates ad hoc endpoints and roundtrip object retrievals

layer between data and client, can transform, aggregate, bridge the gap, without either side having to sacrifice flexibility

single request, no n+1 problem
automatically joins

allows to select subset
allows to alias names

server publishes clear and explicit rules, types
can validate query ahead of time against schema

introspect schema, understand API better, self-documenting

/// spec

a query language and execution engine


///// ?

can save static query on server, just send identifier and variables from client

//// Lee Byron talk

- fast
bundle multiple request and response in one
describes what needs upfront
single round trip
can only get subset of what needs
- robust
response shape guarantees
no crashes because unexpected format
static types, schema
- self-documenting
no manual out-of-date documentation
static analysis locally
- faster development
additional layer, decouples client further from server, aliasing, etc.

no need to version

//// benjie talk

tooling, linting, code generation works with any GraphQL API thanks to schema

//// meta guys talk

layer makes looser coupling, constant endpoint, etc.

separate problems of server from problems of client

/// yelp guy

if field is unexpectedly null, then sets parent to null, walks up the tree until fiends nullable field, never sends null where expected non-null

null in non-null field bubbles up to next nullable field