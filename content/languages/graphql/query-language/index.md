---
title: Query language
author: vwkd
index: 2
tags:
  - graphql
---
# Query language



## Introduction

- language for retrieval (and modification) of information
- beware: "query" is overloaded as synonym for "operation" or "request", is actually one specific operation ⚠️
- beware: in definitions here use `*` to denote optional, `+` to denote list, `""` to denote literal string, other symbols are literally used, ❗️



## Operation

- action that API performs
  - query: read data, R in CRUD
  - mutation: write and read data, CUD+R in CRUD
  - subscription: read data repeatedly without further operations, based on events on server ??
- beware: always returns data, only ever needs single request, i.e. mutation and subscription are "extended queries" 🎉
- JSON-like, shaped like data it returns (if uses JSON)
- beware: don't confuse query language with JSON, format of request vs. response, happen to have similarities but aren't the same, e.g. in query language can give arguments to field, field is like object property without a value, etc. ❗️

```plaintext
OperationDefinition:
  OperationType Name VariableDefinitions* Directives* SelectionSet

OperationType:
  "query"
  "mutation"
  "subscription"
```

- can define multiple operations in a document, name is free to choose by client
- beware: currently can execute only single operation per request, needs to specify operation name with request, makes defining multiple operations useless, see [#375](https://github.com/graphql/graphql-spec/issues/375) ❗️

```graphql
query getUser {
  user(id: 42) {
    name
  }
}

# query getSomethingElse { ... }
```

- shorthand: for single operation without variables and directives can leave out name
- beware: should always use name, shows up in server error logs ❗️

```graphql
query {
  user(id: 42) {
    name
  }
}
```

- shorthand: for single *query* operation without variables and directives can leave out name and operation type

```graphql
{
  user(id: 42) {
    name
  }
}
```

- beware: difference between mutation operation and query operation is just sideeffects, same exact function otherwise, by convention uses verb in name of field ❗️

```graphql
mutation {
  createUser(name: "Peter", age: 42) {
    name
  }
}
```



## Field

- piece of data that can operate on
- corresponds to property in response object
- are always optional, can't require client to query, differs from argument values

```plaintext
SelectionSet:
  { Selection+ }

Selection:
  Field
  FragmentSpread
  InlineFragment

Field:
  Alias* Name Arguments* Directives* SelectionSet*

Alias:
  Name :
```

- can be of composite type or primitive type, see IDL#Types
- if of list type that contains composite type, can query further down and applied to each item of array

```graphql
# query operation with shorthand
{
  user(id: 42) {
    name
    friends(first: 2) {
      name
    }
  }
}
```

```json
// response data
{
  "user": {
    "name": "Peter",
    "friends": [
      {
        "name": "John",
      },
      {
        "name": "Sarah"
      }
    ]
  }
}
```

- operation must specify selection down to fields of primitive type, makes client fully define shape of response, makes adding new fields not break existing consumers 🎉
- property name in response object is field name by default, can use alias to specify custom property name
- needs to use different aliases to query same field multiple times, because property names in response object must be unique

```graphql
# query operation with shorthand
{
  userOne: user(id: 42) {
    name
  }
  
  userTwo: user(id: 21) {
    name
  }
}
```

```json
// response data
{
  "userOne": {
    "name": "Peter"
  },
  "userTwo": {
    "name": "Lara"
  },
}
```

- specifying same field multiple times without differing aliases is merged
if of primitive type then other ones are ignored
if of composite type, if arguments and directives are equal, then look at children, otherwise throw error

```graphql
# query operation with shorthand
{
  user(id: 42) {
    name
    # next one is ignored
    name
  }
}
```

```json
// response data
{
  "user": {
    "name": "Peter"
  }
}
```

```graphql
# query operation with shorthand
{
  user(id: 42) {
    name
  }
  # next one throws error
  user(id: 21) {
    name
  }
}
```



## Argument

- input to field or directive
- can use to input data for mutation operation, usually required arguments
- can use to transform output data, e.g. units, internationalisation, etc.

```plaintext
Arguments:
  ( Argument+ )

Argument:
  Name : Value
```

- beware: needs to specify name of arguments unlike in JS, are unordered since list ❗️

```graphql
# query operation with shorthand
{
  user(id: 42) {
    name
    height(unit: METER)
  }
}
```

- beware: don’t confuse argument with field ❗️

```graphql
# query operation with shorthand
{
  user(id: 42) {
    id
  }
}
```

<!-- todo: maybe call properly "input value"? -->
- notation of values is largely similar to JS, for concrete details see [spec](http://spec.graphql.org/draft/#sec-Input-Values), e.g. block string with `"""`, etc.

```plaintext
Value: 
  Variable
  IntValue
  FloatValue
  StringValue
  BooleanValue
  NullValue
  EnumValue
  ListValue
  ObjectValue
```

- ??? beware: don't confuse `Value` with `Type`, in same position but in query language vs. in IDL, query language defines syntax for each type, IDL defines types that can use ⚠️
one defines input in request while other one defines output in response
- ??? beware: don't confuse argument with field, types in schema define input vs. output, e.g. non-null type of field requires value in response, non-null type of argument requires value in request ⚠️

- argument is optional by default, can make required with non-null type, see IDL#Type

### Variable

<!-- todo: maybe call properly "input value"? -->
- value can be variable, defined at top of operation, scope is body of operation including fragments
- beware: needs to define variable on every operation where it's used, e.g. due to fragment
- value of variable is supplied along with document in request, substituted by engine, if omitted is set to `null`
- can use variable to personalise query on client without needing to change query
- beware: never manipulate request string at runtime, instead use variables for dynamic values, e.g. predictability, separate errors for dynamic values due to type of variable, etc. ❗️
- beware: only non-constant values can be variables, see spec since here hasn't annotated what's constant, e.g. `DefaultValue` of variable itself

```plaintext
Variable:
  $ Name

VariableDefinitions:
  ( VariableDefinition+ )

VariableDefinition:
  Variable : Type DefaultValue* Directives*

DefaultValue:
  = Value
```

<!-- todo: maybe under input type in IDL? -->

```plaintext
Type:
  NamedType
  ListType
  NonNullType

NamedType:
  Name

ListType:
  [ Type ]

NonNullType:
  NamedType !
  ListType !
```

```graphql
query getUser($userId: Int) {
  user(id: $userId) {
    name
  }
}
```

- think of variable like placeholder, instead of directly writing value writes placeholder that is substituted later
- beware: for variable needs to annotate type also in query language, like a leak of IDL into query language, engine needs to verify that type of variable matches type of argument where used, and type of value provided matches type of variable
- type of variable needs to be input type, see IDL#Type



## Directive

- special instructions to engine for operation, field (or fragment), or variable or schema engine
- can use to alter execution behavior, e.g. conditionally include/skip field based on its value or value of variable
- can use to alter schema behavior, e.g. mark field as deprecated
- order of directives is significant

```plaintext
Directives:
  Directive+

Directive:
  @ Name Arguments*
```

- beware: list of directives has no surrounding symbol, e.g. no `{}`, `()`, etc. ❗️
- built-in directives in any engine:
  - `@include(if: Boolean!)` on field (or fragment): includes field if condition is `true`
  - `@skip(if: Boolean!)` on field (or fragment): skips field if condition is `true`
- beware: if specifies both `@include` and `@skip` on same field (or fragment), field is included only if both directives are satisfied, i.e. condition in `@skip` false and in `@include` true



## Fragment

(read beforehand IDL to understand what types are)

- set of fields on a specific type
- can reuse and/or use conditionally based on type of parent field and/or directive
- used with spread syntax, e.g. like object spread in JS
- is included if type in type condition matches (or is implemented by) type of parent field it's in, see IDL#Types

```plaintext
FragmentSpread:
  ...FragmentName Directives*

FragmentDefinition:
  "fragment" FragmentName TypeCondition Directives* SelectionSet

FragmentName:
  Name

TypeCondition:
  "on" NamedType
```

- beware: `FragmentName` may not be "on" otherwise parser can't distinguish `FragmentSpread` from `InlineFragment` ❗️
- can use to include set of fields at multiple places

```graphql
# query operation with shorthand
{
  user(id: 42) {
    ...userInfo
    friends(first: 2) {
      ...userInfo
    }
  }
}

fragment userInfo on User {
  name
}
```

- can use to include set of fields conditionally based on type, see IDL#Types
- beware: type of parent field must contain that type (e.g. be of a union type), otherwise throws error, i.e. can't just include fragment anywhere

```graphql
# query operation with shorthand
{
  # union Profile = User | Page
  profiles(first: 2) {
    ...userAge
    ...pageUrl
  }
}

fragment userAge on User {
  name
}

fragment pageUrl on Page {
  title
}
```

- can use to include set of fields conditionally based on directive, e.g. based on variable
??? TODO: what's the difference between directive in FragmentSpread or in FragmentDefinition

```graphql
query getUser ($showAge: Boolean) {
  user(id: 42) {
    name
    ...userAge @include(if: $showAge)
  }
}

fragment userAge on User {
  age
}
```

- if isn't reusable can define inline fragment, i.e. only used for conditionally inclusion based on type of parent field and/or directive
- if inline fragment is only based on directive, can leave out type condition, type condition defaults to type of parent field, i.e. matches every type

```plaintext
InlineFragment:
  ...TypeCondition* Directives* SelectionSet
```

```graphql
# query operation with shorthand
{
  profiles(first: 2) {
    ...on User {
      name
    }
    ...on Page {
      title
    }
  }
}
```

```graphql
query getUser ($showAge: Boolean) {
  user(id: 42) {
    name
    ...@include(if: $showAge) {
      age
    }
  }
}
```

- fragment can't use other fragment that creates a cyclic reference, e.g. itself



## Resources

- [GraphQL - Specification 11/2020](http://spec.graphql.org/draft/)