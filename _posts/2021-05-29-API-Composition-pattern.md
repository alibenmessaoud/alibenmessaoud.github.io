---
layout: post
title: API Composition Pattern
tags: pattern best-practice cqrs api-gateway
---

When we write code to query multiple service endpoints, we have two options to follow: 1/ the complex but the powerful option is CQRS or 2/ the simple one is API Composition. Let's explore the second option!

The idea behind the API Composition Pattern is to query all the services and combine the results.

The main components in this pattern are the API composer and the other service providers. The API composer knows the other services to request and implements the query operations by invoking the providers and combining the results.

```asciiarmor
                 QUERY
                   │
           ┌───────▼──────┐
       ┌───┤ API COMPOSER ├───┐
       │   └──────────────┘   │
       │                      │
       │                      │
┌──────▼─────┐         ┌──────▼─────┐
│ SERVICE #1 │   ...   │ SERVICE #N │
└────────────┘         └────────────┘
```

The composer can be something on the frontend or backend. You may ask if this pattern is not the API Gateway Pattern?

Well, it is not the same thing.

The principal goal of an API gateway is to be the single entry point for several services, and it routes requests of clients to these services. It does not contain any business logic of one particular service.

Even though the API composition handles multiple service results, this pattern still remains free of any domain logic. See it as an aggregate of the services (behind the API gateway) to handle inbound queries.

> The Composer and the Gateway patterns can be composed/combined for more complex use cases. 

Also, we can have multiple intermediate API composers (or API composition layers).

The API composition pattern introduces some more advantages. It helps to replace legacy systems over time and create a consistent design of the overall API.

On the other hand, it is not fit to handle large and in-memory joins of datasets. Also, composing too much can lead to an increase in the learning curve of the system.