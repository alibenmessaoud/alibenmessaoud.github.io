---
layout: post
title: Single Responsibility Principle
tags: best-practice solid
---

In this article, we will see the Single Responsibility Principle (SRP), which is the “S” in the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles.

> “The Single Responsibility Principle (SRP) states that each software module should have one and only one reason to change.” — Uncle Bob Martin, [The Single Responsibility Principle](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)

Sounds bizarre!? But when you read "single responsibility", it means a thing must do one single job. Let’s look again and try to decrypt the meaning.

## Responsibilities

Responsibilities can be present on different levels of the software system: methods, classes, modules. Notice, the SRP says that each component should have a single responsibility. As an example, a method has a responsibility or let me say a flow of control, and it can delegate some of its tasks to other methods to achieve a specific goal. The same theory of the flow of control can be applied to the class level and also the module level. Hence, the responsibilities size or scope can be different at each level. 

But uncle Bob said each software module should have one and only one reason to change. Confused?

Let’s go back to the main idea of using the SRP. Clearly, it’s to enhance the overall quality of the codebase.

Therefore, what will eventually help us to use the Single Responsibility Principle efficiently is making it clearer for us how SRP influences the quality of the code and the system. In other words, how to manage and organize the code of the system to guarantee sustainability? Which means the same behavior after each new feature!

Imagine you got a request and you did your job. However, I am sure that you don't want to hear that the application is down because of you, and because of a change completely unrelated to the changes they requested had caused the downtime. Nobody wants to be in that situation.

So the definition of responsibility is not about doing a job only. No! The responsibility goes far and it includes the change and the impact on the other parts of the system. 

So, to define the responsibility of each part of the software we need to take into account:

- the responsibility itself
- the non-functional requirements
- the code 
- the future
- people
- changes request

Responsibilities can be set by defining borders and scopes of each part of the system. In other words, it is by splitting or merging components while we observe that the software quality is in continuous improvement.

This is the reason we *separate concerns*.

Another wording for the Single Responsibility Principle is:

> “Gather together the things that change for the same reasons. Separate those things that change for different reasons.” — Uncle Bob Martin, [The Single Responsibility Principle](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)

So to be clear, it is about cohesion and coupling. 

We want to rise the cohesion between pieces that change for the same reasons, and we want to reduce the coupling between the other pieces that change for different purposes.

## The impact of SRP on code

The impact of the SRP on different software qualities: 

- Understandability and Learning Curve: Splitting responsibilities helps to faster mastering
- Flexibility: Combining independent components becomes easier
- Reusability: Reusing components with single narrow responsibility becomes easier
- Testability: Writing and maintaining tests for methods and classes with focused concerns becomes easier
- Debuggability: Covering tests can help to locate bugs
- Observability and Operability: Profiling becomes more informative to enhance performance.
- Reliability: Less external communications more failure-free software operation
- Code Size: Focused responsibilities means less of code to write
- Performance: the size of services might significantly impact the efficiency of the system

## Conclusion

The SRP is simple but hides many messages so it is easy to go in the wrong way. The SRP can be applied to all levels of the software. It is for the team to decide how to define responsibilities and the size of each part on each level of the software. Also, they must pay extra attention to cohesion. With this, they can define a set of standards to follow.