---
layout: post
title: DDD Series
---

So a common glossary of technical and business terms to be used in the organization is very important to succeed in the project.

During my career as a software engineer, I was a member of many teams where domain experts are the holder of knowledge of their domain, and architects are the expert of the technical concerns in a digital project. 

Each one of them speaks a language, a language that is different from the spoken by the developers. As a developer, we are the responsible of translating each word from these actors to UIs and interactions. So, the resulted system will take the shape of all conversations bad or good it was. In this quick article, we will see DDD and how it helps designing software effectively, in-line with the business requirements, and by separating the infrastructural concerns from the domain logic.



Unlike xDD approaches, which provide a framework to write good quality of code, DDD concerns software design. 

DDD is not a method for designing software but it shows how to design software with a focus on the business domain.

Eric Evans, introduced the concept in 2004, in his book *Domain-Driven Design: Tackling Complexity in the Heart of Software*. 

According to DDD: Tackling Complexity in the Heart of Software book of Eric Evans, DDD focuses on three principles:

- Core `domain` and `domain logic`
- Models of the `domain`
- Collaboration between `domain expert` and developers

Also, one of the key advantages of DDD is that it allows the team to create a model and present it to experts of the domain but also other actors with data model entities, functional specifications, and process modeling.

Moreover, Eric Evans stated some common terms that are useful when describing and discussing DDD practices:

- **Context**: in which a word or statement appears.
- **Model**: A system of abstractions that describes a `domain`
- **Ubiquitous Language**: A common language around the `domain model` and used by all team members
- **Bounded Context**: A description of a boundary in which a model is defined and applicable