---
title: Cloud
author: vwkd
index: 10
tags:
  - the-web
---
# Cloud

<!-- ToDo: finish -->



## Introduction

- on-demand shared computer resources
- on network of remote servers on the Internet
- provider maintains physical infrastructure, e.g. reliability, scalability, security, performance, etc.
- beware: misnomer, "it's just someone else's computer" ❗️



## Service models

- usually pay-per-time
- from IaaS to FaaS can reduce idle time

### Infrastructure as a service (IaaS)

- provision of an operating system, e.g. processing, storage, networking, etc.
- provider manages (physical) infrastructure
- e.g. Google Compute Engine

### Platform as a service (PaaS)

- provision of a platform, e.g. runtime, database, etc.
- provider manages infrastructure
- e.g. Google App Engine

### Function as a service (FaaS)

- provision of code execution environment, e.g. HTTP middleware
- provider manages the platform and infrastructure
- e.g. Google Cloud Functions

- triggered by events, e.g. HTTP API, connected services, etc.
- beware: often time limit on execution before is stopped, e.g. 5 minutes, etc.
- beware: also called "serverless" because server is abstracted away completely, although there is still a server ❗️

declaratively establishes relationships between individual functions of what otherwise would have been a single server
can use to disassemble server into its individual functions, each becomes independently scalable, no idle time
no idle time

can use for back-end HTTP API



## Microservices

<!-- todo: finish 

security and scaling
-->

- should be independent, no tight coupling, such that can scale independently, i.e. no Authentication in each
<!-- todo: replace that infact AuthZ can't separate from AuthN -->
- Authentication is own microservice, while authorization is part of every microservice  
  just can create central MS for central role-to-permission mapping management over all microservices, doesn't have to change things manually
- should use event bus between microservices, speaker doesn't need to know who wants to listen, loose coupling
- beware: microservice itself is easy part, cloud in between is hard part, e.g. message could be lost anywhere on wire ❗️
- beware: if uses event bus, needs to sign messages, otherwise attacker that got access to one application can control rest through sending events ❗️
- beware: use HTTPS between microservices, otherwise attacker in network can control any service❗️
- credentials between microservices, don't accept incoming traffic from anyone
- establish trust boundaries between microservices, user data may not enter trust boundary without being validated
- every microservice checks authorization of any request for operation, keeps logic contained at source instead of in separate microservice that needs to keep up to date
- beware: needs to pass on user state between microservices such that can check authorization ❗️
- beware: if deletes user, needs to delete data in all APIs, can use event stream between microservices, should handle duplicate deletion event gracefully ❗️
- should have one MS that handles users (e.g. AS), and all others just reference users by userID, however when user is deleted needs to loop over all MS and delete relevant data, don't leave orphaned data otherwise storage need grows indefinitely

### Examples of Microservices

- media hosting: Cloudinary, etc.
- comments: Disqus, etc.
- contact form: Formcarry, Netlify Forms, etc.
- search: Algolia
- authentication: Auth0, Orka
- payment: Shopify, Stripe
- CMS: Netlify CMS, etc.



## Resources

- [NIST - The NIST Definition of Cloud Computing](https://doi.org/10.6028/NIST.SP.800-145)