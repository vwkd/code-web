---
title: Pagination
author: vwkd
index: 4
tags:
  - languages
  - graphql
---

- splitting list into chunks
- beware: "page" is historical term where chunks were used for book-like pages, think Google
- allows client to fetch only part of list, resume where left off
- assume sorted list, list adds / deletes items only at ends, e.g. sorted by creation date of item
- beware: can not add / delete data in middle of list, otherwise duplicate / skipped chunks ❗️



## Offset

- offset from beginning (or end) of list

```graphql
{
  users(first: 2, offset: 2) {
    name
  }
}
```

- problem: can only add data to opposite end of list from which offset, otherwise duplicate / skipped chunks
- use if list adds / deletes items only to / from opposite end of offset, never other end or middle



## Cursor

- pointer to item in list
- uses unique property of item that doesn't change, e.g. `id` field
- make cursor opaque to not leak implementation details, otherwise can't change in future, e.g. base64 encode of `id` field

```graphql
# beware: not functional yet, read further
{
  users(first: 2, after: $userCursor) {
    name
  }
}
```

- needs to communicate cursor, can't be field of list object since meta property of connection
- need to introduce intermediate object in hierarchy, corresponds to relationship between two nodes in graph
- beware: pagination is "ugly", because prevents that all objects correspond to nodes in graph ❗️
- "connection" object, contains `pageInfo` object with cursor and last page flag
- connection can have metadata fields, e.g. `totalCount` of connections

```graphql
{
  usersConnection(first: 2, after: $userCursor) {
    totalCount
    users {
        name
    }
    pageInfo {
        endCursor
        hasNextPage
    }
}
```

- need to introduce another intermediate object because wants cursor for individual item, allows to continue paging through list from any item instead of only from end of chunk
- edges list, each edge contains own cursor
- edge can have metadata fields, e.g. date `since` edge exists
- beware: final object is singular `node` object, since edge list is isomorph to object list

```graphql
{
  usersConnection(first: 2, after: $userCursor) {
    totalCount
    edges {
        node {
          name
        }
        cursor
        since
    }
    pageInfo {
        endCursor
        hasNextPage
    }
}
```

- can still provide list object on connection object as convenience for when doesn't need cursor or metadata of each edge

```graphql
{
  usersConnection(first: 2, after: $userCursor) {
    totalCount
    edges {
        node {
          name
        }
        cursor
        since
    }
    users {
        name
    }
    pageInfo {
        endCursor
        hasNextPage
    }
}
```

```graphql
type Query {
  usersConnection(
    first: Int,
    last: Int,
    after: String,
    before: String
  ): UsersConnection
}

type UsersConnection {
  totalCount: Int
  edges: [UsersEdge]
  users: [User]
  pageInfo: PageInfo!
}

type UsersEdge {
  node: User
  cursor: String!
  since: Date
}
```

- beware: could also name `users` what here is `usersConnection` and `nodes` what here is `users`, e.g. GitHub ❗️



## Resources

- [GraphQL - Pagination](https://graphql.org/learn/pagination)
- [Caleb Meredith - Explaining GraphQL Connections](https://www.apollographql.com/blog/explaining-graphql-connections-c48b7c3d6976/)
- [GraphQL Cursor Connections Specification](https://relay.dev/graphql/connections.htm)