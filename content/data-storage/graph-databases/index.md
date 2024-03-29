---
title: Graph databases
author: vwkd
index: 3
tags:
  - data-storage
---

<!-- ToDo: Finish -->
<!-- todo: when to use node vs. attribute
https://stackoverflow.com/questions/52369909/neo4j-node-versus-node-property

- Need to traverse relationship often ?
- Avoid supermodels
 -->

## Graph

- set of vertices and edges, nodes and relationships
- can represent connected data



## Introduction

- stores data in a graph, closer to real world
- relationships are first-class citizens, just as much as entities
- schema-free: data can have variable structure, no two nodes need to have the same attribute or relationship types
- no additional artificial constructs just for implementation, e.g. foreign keys, join tables, etc.
- scales better for large amounts of data, allows to do more powerful analysis, e.g. pattern matching



## Graph model

A labeled property graph:
- nodes and relationships are naked and have properties
- Relationships connect exactly two nodes and are directed



- in ER model entity is a node, relationship is an edge
- ER model is a graph
- , i.e. database stores multiple instances of abstract types, ER model shows structure

use nodes for entities with identity
attributes for properties of the entities
relationships to connect entities
attributes for relationship to specify relationship
label nodes to give roles, group entities

- every relationship can have only one start and end node


- entity is a node, relationship is an edge, i.e. database stores multiple instances of abstract types, ER model shows structure

Labels are used to shape the domain by grouping nodes into sets where all nodes that have a certain label belongs to the same set.
A node can have zero to many labels. ????


Label of nodes allows indexing of attributes
Label of nodes allows constraints on attributes
e.g. all Person must have a name property, name of Person must be unique, etc.

labels group nodes into sets, relationship can have only one label
Relationships always have a direction

attributes have a data type, e.g. string, number, array, etc.

Node labels, relationship types and properties are case sensitive

Naming conventions

| Thing | Convention | Example |
| - | - | - |
| Node label | CamelCase | `VehicleOwner` |
| Relationship type | UPPER_CASE | `OWNS_VEHICLE` |
| Attribute name | camelCase | `firstName` |



## Modeling tips

- make intermediary nodes if relationship contains standalone information, can use intermediary nodes to build more relationships, e.g. instead of `User reviews Product` use `User writes Review reviews Product`, can add review-specific details to `Review` node, e.g. `User sends Email to User` instead of `User emails User`
link off from relationship to intermediary nodes
- use intermediary nodes if wants to connect more than two nodes, e.g. add details to relationship, n-ary relationship, etc.
e.g. `User works at Company` and attach `as Role`, or `Customer buys Product` and attach `for Price` and `using Payment`
- keep attributes on nodes small, put attributes into own nodes, e.g. person with single attribute name, relationships lives in city, has telephone number etc.
-> can't sort graph by attributes, but can visualise how many customers live in city
everything that wants to be able to aggregate by should be an entity, instead of an attribute
- When attributes of a node or separate node connected to node
e.g. address is property of User or separate node connected to user?
-> if attribute value is a complex value type pull out into separate node with attributes, e.g. address
-> if want to group, by this attribute, start traversing graph at this attribute, e.g. tags, skills
-> if attribute is in certain relationship with node, e.g. `award` attribute to which wants to add date, etc.

- Make relationship names precise, otherwise query needs to go through many nodes, e.g. not `has`, but `has_skill`

- Make attributes standalone nodes, if they are important entities, want relationships between them, wants to categorise by it, e.g. `MATCH Person has Skill xyz`, instead of `MATCH Person {skill: xzy}`


Use nodes if wants be able to group by
Use attributes otherwise

Use nodes if wants to connect two more than two entities
Use relationship otherwise

Use fine grained relationships names, checking attributes on target mode is more expensive, e.g. Person located_home Address instead of Person located Address `{type: home}`
Eliminates graph travels, faster performance
Put complex attributes with multiple attributes in own nodes, e.g. Address




## Time-based versioning

- version graph over time
- needs to encode time using separate state nodes for time-less entities

- separate entities from state


- can update without deleting anything, can go back in time, e.g. price history
- can also version relationships, but often wants to version only entities
- include verification, e.g. timestamp history must chain continously without gaps
- can put a label `CurrentState` on the current `*State`
- only version what you need to keep track off, introduces complexity

- How to implement time in the graph, versions of the graph at points in time, e.g. prices a week ago, number of friends a year ago, etc.
need to separate state from structure, need to be able to version independently from each other, e.g. structure node changes or state node changes
purely additive, never delete
use "entity nodes" to link to first and last version of that node, each intermediary node links to next,
add `to` and `from` properties to every relationship to limit validness, use EPOCH time, use max EPOCH for `from` even if unlimited to store it together with other attribute on disk
alternatively attach linked list to structure node with versions, attach next state node with timestamp
beware: much more data, need to add every change, queries become more complex
add state node for every attribute except id

Versioning imagine graph model, use only when needed, complicates every query


## Query

- request for information from a database
- 
query returns collection of matches, result iterates over matches !!!!!!!


## Storage

a graph database has native processing capabilities if it exhibits a property called index-free adjacency
index-free adjacency
- connected nodes physically “point” to each other in the database
- each node maintains direct references to its adjacent nodes
- Doesn’t need global index, query times are independent of size of graph, only proportional to amount of nodes involved





## Resources

- [Wikipedia - Graph database](https://en.wikipedia.org/wiki/Graph_database)
- [David Bechberger - A Skeptics Guide to Graph Databases](https://www.youtube.com/watch?v=yOYodfN84N4)
- [Ian Robinson - Tips and Tricks for Graph Data Modeling](https://www.youtube.com/watch?v=78r0MgH0u0w)