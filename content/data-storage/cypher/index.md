---
title: Cypher
author: vwkd
index: 4
tags:
  - data-storage
---
# Cypher

<!-- ToDo: Finish -->



## Introduction

- query language for Neo4j
- matches pattern


## Syntax

- see Neo4j
- e.g. `(:Person) -[:LIVES_IN]-> (:City) -[:PART_OF]-> (:Country)`


## Clauses

| clause | description | note |
| - | - | - |
| `CREATE` | creates pattern | |
| `MATCH` | matches pattern | |
| `MERGE` | matches pattern or creates it if not existent | |
| `WHERE` | filter a match ??, can use boolean expressions, predicates, and `OR`, `AND`, `XOR`, `NOT` |
| `RETURN` | returns variable???, iterates through matches, one row per match, can use aggregation functions,  `DISTINCT`, `AS` | |
| `ORDER BY` | orders returned result, can only use after `RETURN`, can use `SKIP`, `LIMIT`, `DESC`, `ASC`,   |

- beware: a `CREATE` after a `MATCH` is executed once for each match ❗️
- with `MERGE` can use `ON CREATE SET` to create further things conditionally
- a `MERGE` is more costly than a `CREATE`, since it needs to query the graph first, use cautiously ❗️
- beware: a `MERGE` either matches a pattern or creates it, it never does both ❗️

create and match patterns in the graph
filter, project, sort, or paginate results
compose partial statements

| `UNION` | combines the results of two statements with same result structure | returned columns must be aliased in the same way in all the sub-clauses |
| `WITH` | pipes result to next clause, similar to `RETURN` except it doesn't finish the query, can use same expressions as in `RETURN`, only columns declared here are available in subsequent clauses | all columns must be aliased |