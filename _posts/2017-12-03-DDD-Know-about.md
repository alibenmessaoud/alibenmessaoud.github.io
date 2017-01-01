---
layout: post
title: DDD - Know About
tags: ddd architecture ddd-series
---

To define DDD, we should first learn by what does it means "domain" in this context?

> Eric Evans coined the term DDD in Tackling Complexity in the Heart of Software, published in 2004.

Of course, we create software, and we must deeply understand the domain of that built software. Right? 

So when we implement functionalities in a specific **domain** like HR or Banking, we create a **model** of that domain.

A **domain** is a sphere of activity around which the application logic revolves.

A **model** is an abstraction, or a blueprint, of the **domain**.

> If an application is specifically CRUD-oriented, DDD would be YAGNI. 
>
> ```
>        /\
>        ||
>       CRUD
>        ||
>   Active Record
>        ||
>       Data
>        ||
>    Domain Model
>        ||
>       DDD
>        ||
>        \/
> ```
>
> Otherwise, if the application is really task-oriented, catching a specific intent of the user, DDD might really help.

So, DDD's goals are to discover the model of a domain, and we can achieve this goal by speaking with the experts of the domain. Furthermore, DDD involves a developer right from the initial stage.

> Designing a model is not rocket science, but it does take a lot of effort, refinement, and input from domain experts.

Then we need to take the discovered knowledge and translate them into model, code, rules, and logic. 

The domain model provides a high-level view of a solution's architecture, and the code implementation gives the domain model a life as a working model.

And finally, we need to protect the domain and its logic from infrastructural concerns. 

Moreover, by "driven-design", we mean "mindset to adopt" rather than a list of design patterns to apply.

> DDD makes design and development work together.

Want to know more? Here some books to read:

- The blue book, Eric Evans, 2004

- The red book, Vaughn Vernon, 2013

- The green book, Vaughn Vernon, 2016

- The white book, Scott Wlaschin, 2018