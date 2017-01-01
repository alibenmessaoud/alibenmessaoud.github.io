---
layout: post
title: DDD - Building Blocks
tags: ddd architecture ddd-series
---

DDD fundamentals can be divided into two groups: building blocks and strategic design. In this article we will look at the building blocks such as ubiquitous language, multilayered architecture, and the artifacts, and  explain the usage and importance of the building blocks of DDD.

## Ubiquitous Language

As seen before, designing a model is the result of collaboration of multiple partiers such as domain experts and developers. Domain experts do not know too much about the code as the developers do not know all the business detail. These two parties must have a common language to communicate with. 

DDD makes sense to use ubiquitous language. 

The developers and the domain experts must use the ubiquitous language in diagrams, descriptions, presentations, and meetings. Therefore, it removes misunderstanding, and communication gaps between them.

## Multilayered architecture

It contains four layers:

1. Presentation layer: It provides the user interface for interaction and the display of information
2. Application layer: It maintains and coordinates the overall flow of the product
3. Domain layer: : It contains domain information and business logic. It keeps domain-related code separate from other layers
4. Infrastructure layer: It is responsible for communication between the other layers, for example, interaction with databases, message brokers, file systems, and so on.

## Artifacts

There are seven different artifacts used in DDD to express, create, and retrieve domain models:

1. Entities: 

   - mutable, remain the same throughout the different states of the product

   - entity is uniquely identified by thread of continuity and by an ID rather than by an attribute. Two entities have the same ID, both are considered equal regardless what attributes they have

   - creating entities means creating an identity (pk, genid, ck, etc.)

   - defined at the initial stage of the modeling process
2. Value objects

   - immutable
   - have no identity (ID) 
   - simple or composite values that have a business meaning
   - can be compared on the basis of their collective state
3. Services

   - provide behavior to the domain that does not belong to a single entity or value object
   - part of the domain layer that does not have any internal state, and hence, they are stateless
   - may also exist in other layers and it is very important to keep domain-layer services isolated
4. Aggregates

   - tactical pattern
   - avoid bulk data loading or lazy loading
   - a group of business objects which always need to be consistent
   - responsible for controlling access to all of the members of its aggregate
   - defines ownership and boundaries and avoid violate business constraints
   - aggregate attributes can be immutable objects
5. A repository
   - part of the domain model
   - interacts with infrastructures such as the database or filesystem
6. A factory
   - helps to create complex objects, or an aggregate
   - Factories and repositories are in some way related to each other, as both refer to domain objects
7. A module
   - form of code organization
   - tell the story of the system on a higher level
