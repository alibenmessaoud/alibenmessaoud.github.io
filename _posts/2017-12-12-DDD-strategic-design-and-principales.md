---
layout: post
title: DDD - Strategic Design and Principles
tags: ddd architecture ddd-series
---

DDD fundamentals can be divided into two groups: building blocks and strategic design. In this article we will look at the Strategic design and principles such as bounded context, continuous integration context map, and  explain the usage and importance of these concepts of DDD.

In large companies with several, separate departments and leadership team, maintaining the integrity of the domain model is not an easy task. In such cases, working on a unified model is not the solution. So creating  sub-models with the predefined accurate relationships and contracts is the solution. There are various principles that can be followed to maintain the integrity of a domain model, and these are listed as follows:

## Bounded context

It is difficult to maintain the code of all the combined models particularly when they are different. More information about these models will be needed. To deal with this problem, contexts are the solution to keep the meaning of each concept in the models. Why? Because every team will have their own definitions and localize that definition. For example, a shared class will bring too much overhead and become excessively costly. 

A bounded context clarifies, and defines the responsibility of the model. It ensures the domain will not be distracted from the outside. Each model must have a defined context within a sub-domain, and every context defines boundaries. This kind of boundary is based on concrete technical implementation and it must not have linguistic ambiguity because, first and foremost, it's a linguistic boundary.

## Continuous integration

When we design and develop systems using DDD and agile methodologies, domain models are not designed fully before coding starts. Moreover, its elements evolve over a period of time with continuous improvements. Therefore, continuous integration is one of the key reasons for development today. In continuous integration, code is merged frequently to avoid any breaks and issues with the domain model. Merged code not only gets deployed, but it is also tested on a regular basis. Moreover, organizations put more emphasis on the automation of continuous integration.

## Context map

The context map helps to understand the overall picture of a large enterprise application. It shows how many bounded contexts are present in the enterprise model, and how they are interrelated.

Let's take an example. Imagine we have **Table1** and **Table2** both appear in the **Table Reservation Context** and also in the **Restaurant Ledger Context**. The interesting thing is that **Table1** and **Table2** have their own respective concept in each bounded context. Here, ubiquitous language is used to name the bounded context **table reservation** and **restaurant ledger**.

### Shared kernel

As the name suggests, one part of the bounded context is shared with the other's bounded context. For example, the **Restaurant** entity can be shared between the **Table Reservation Context** and the **Restaurant Ledger Context**.

### Customer-supplier

It is a pattern and represents the relationship between two bounded contexts, when the output of one bounded context is required for the other bounded context. That is, one supplies the information to the other (known as the customer), who consumes the information.

Both customer and supplier teams should meet regularly to establish a contract and form the right protocol to communicate with each other.

### Conformist

This pattern is a form of customer-supplier context map. Here one parts needs to provide the contract and information while the other needs to use it. If the supplier provided information that is partial worth, then the customer could use the interface or translators to consume the supplier-provided information with the customer's own models.

### Anti-corruption layer

This layer remains part of a domain that interacts with external systems, or their own legacy systems. It consumes data from external systems and uses external system data in the domain model without affecting the integrity and originality of the domain model.

For the most part, a service can be used as an anti-corruption layer that may use a façade pattern with an adapter and translator to consume external domain data within the internal model.

### Separate ways

This pattern become important when a large enterprise application of a large domain is made of a large sub-models that can work independently. 

The idea is to created models that have no relationship and can be separated in small applications. These small applications become a single application when merged together and sit on top of all sub-models.

### Open Host Service

It is about a translation layer when two sub-models interact with each other. The Open Host Service is required if more than one sub-model interacts with external systems.

### Distillation

It is a process that filters out information that is not required, and keeps only meaningful information. It helps to identify the core domain and the essential concepts for the business domain. It also helps to filter out generic concepts until getting the core domain concept.