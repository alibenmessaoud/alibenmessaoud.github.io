---
layout: post
title: DDD - Architectures
tags: ddd architecture layered hexagonal reactive ddd-series
---

DDD is not architecture. DDD is an approach to building software with a focus on the domain. Moreover, by "driven-design", we mean mindset to adopt so we can build domain models in the right way and with the intention of solving or improving some aspect of business.

When implementing DDD, we will have to define/design an architecture at some point. This refers to the style of architecture we will use. As said before, DDD is not an architecture and we can use different styles of architecture (Onion, Hexagonal, Layered, CQRS, Event-driven, Reactive, etc.) to apply DDD.

Here I briefly present some of the architectural possibilities, models and patterns to be used in a DDD project:

- Layered: is the most popular and most used architecture. Layers are a form of separation of responsibilities where an *(n)th* layer can access only the *(n-1)th* layer.
- Hexagonal: known as Ports and Adapters. Ports are the interfaces and Adapters are the implementations. There are no layers. You have the application, the ports, and the adapters. With the Dependency Inversion Principle, the domain layer became the central layer that does not depend on any other layer.
- CQRS: Command Query Responsibility Segregation. It separates reading and writing from data, where, Query is for reading and Command for writing data.
- Event-driven: uses Producers of events that generate a flow of events, and Consumers of events that listen to events.
- Reactive: used to create distributed and concurrent applications.
- Onion: uses layers, with the dependencies always pointing inwards, i.e., a layer can use any of the layers inside it. The inner layer is Domain Model and the outer is infrastructure, but the number of layers between may vary.

DDD can assist to build software and solve problems and it is possible to use a specific architecture in each context. For example, we can use a Reactive architecture in context A and Hexagonal Architecture in context B.



