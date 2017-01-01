---
layout: post
title: Difference between CQRS vs CQS
tags: cqs cqrs 
---

CQS (Command Query Separation) and CQRS (Command Query Responsibility Segregation) are very much related. You can think of CQS as being at the class or component level, while CQRS is more at the bounded context level.

I tend to think of CQS as being at the micro level, and CQRS at the macro level.

CQS prescribes separate methods for querying from or writing to a model: the query doesn't mutate state, while the command mutates state but does not have a return value. It was devised by Bertrand Meyer as part of his pioneering work on the Eiffel programming language.

CQRS prescribes a similar approach, except it's more of a path through your system. A query request takes a separate path from a command. The query returns data without altering the underlying system; the command alters the system but does not return data.

Greg Young put together a [pretty thorough write-up](https://cqrs.wordpress.com/documents/cqrs-introduction/) of what CQRS is some years back, and goes into how CQRS is an evolution of CQS. This document introduced me to CQRS some years ago, and I find it a still very useful reference document.







- **CQS** is about Command and Queries. It *doesn't care about the model*. You have somehow separated services for reading data, and others for writing data.
- **CQRS** is about separate *models* for writes and reads. Of course, usage of write model often requires reading something to fulfill business logic, but you can only do reads on read model. Separate Databases are state of the art. But imagine single DB with separate models for read and writes modeled in ORM. It's very often good enough.




This is an old question but I am going to take a shot at answering it. I do not often answer questions on StackOverflow, so please forgive me if I do something outside the bounds of community in terms of linking to things, writing a long answer, etc.

There are many differences between CQRS and CQS however CQRS *uses* CQS inside of its definition! Let's start with defining the two and then we can discuss differences.

CQS defines two types of messages depending on their return value: no return value (void) specifies this is a Command; a return value (non-void) specifies this method is a Query.

- Commands change information
- Queries return information

Commands change state. Queries do not.

Now for CQRS, which uses the same definition as CQS for Commands and Queries. What CQRS says is that we do not want **one object** with Command and Query methods. Instead we want **two objects**: one with all the Commands and one with all the Queries.

The idea overall is very simple; it's everything *after* doing this where things become interesting. There are numerous talks online, of me discussing some of the attributes associated (sorry way to much to type here!).











Meier defends that, as a principle, **we should not have methods that both change data and return data**. So we have two types of methods:

1. **Queries**: Return data but do not change data, therefore having no side effects;
2. **Commands**: Change data but do not return data.

Bertrand Meyer in his book “Object Oriented Software Construction“ (1988)

In other words, *asking a question should not change the answer and doing something should not give back an answer,* which also helps to respect the Single Responsibility Principle.



The main idea of the Command Pattern is to move us away from a data-centric application and into a process-centric application, who has domain knowledge and application processes knowledge. (As explained earlier, using commands we shift the application from a data-centric design to a behavioural design, which comes in line with Domain Driven Design.)

In practice, this means that instead of having a user execute a “CreateUser” action, followed by an “ActivateUser” action, and a “SendUserCreatedEmail” action, we would have the user simply execute a “RegisterUser” command which would execute the previously mentioned three actions as an encapsulated business process.

Technically, the Command Pattern will encapsulate all that is needed to execute some action or sequence of actions.

What we can do, to solve the mentioned Command Pattern limitation, is to apply one of the oldest principles in OO: **separate what changes from what doesn’t change**.





https://herbertograca.com/2017/10/19/from-cqs-to-cqrs/













## Conclusion

By using CQRS we can completely separate the read model from the write model, allowing us to have optimised read and write operations. This adds to performance, but also to the clarity and simplicity of the codebase, to the ability of the codebase to reflect the domain, and to the maintainability of the codebase.

Again, it’s all about encapsulation, low coupling, high cohesion, and the Single Responsibility Principle.

Nevertheless, it’s good to keep in mind that although CQRS provides for a design style and several technical solutions that can make an application very robust, that doesn’t mean that all applications should be built this way: We should use what we need, when we need it.