---
layout: post
title: Designing a Java API
tags: api design java contract
---

API is the acronym for Application Programming Interface, and it is a thing that developers use to create features.

Also, it a software intermediary that allows two applications to talk to each other. It establishes a contract between developers, the API consumers, and the software designers, the API providers. 
In the end, providers can use/consume other APIs to provide APIs. 
In this sense, we're all API designers.

We are not creating data silos or isolated systems. Components of APIs or pieces of a system cooperate with other APIs or pieces. These systems become more useful only when they interact with other software written by other developers. 

That's why every developer should know the characteristics of good APIs and how to achieve them.

First, a good API should be easy to read and understand how it works without reading its documentation. The use of consistent naming and conventions is the crucial thing to make it understandable. Even in Java API, there are situations where this suggestion hasn't been followed.

> For example, skip(n) is used to skip the first n items of a Stream. But the method used to skip elements based on predicate was called dropWhile(p). I think it was better to name it skipWhile(p). 

Second, keeping an API minimal is another way to make it easy to use and helps to reduce the concepts to learn. Moreover, updates and maintenance become easier over time. Again, even in Java API, there are situations where this suggestion hasn't been followed.

> The author of the Optional API introduced the static factory method of(object). Unfortunately, this method throws a NullPointerException when invoked with null. So the author added ofNullable(object) to fix the problem when called with null. But, it would be better from the beginning to implement of(object) correctly.

Third, the other helpful hints that could enhance an API are: Interface segregation principle and Fluent APIs. Java Streams API is an excellent example. Moreover, never return null; use empty collections and Optional instead; For the exceptions, limit their usage and possibly avoid checked ones. 

Fourth, avoid long lists of arguments in the methods, particularly of the same type; use the weakest possible type, and keep the arguments in a consistent order among different overloads. 

Fifth, an API must be self-explanatory, but also it requires to be documented clearly. Additionally, writing tests and examples and reviewing them with users can reduce unclear intentions and unnecessary code or features.

Finally, designing an API is an iterative task, and dogfooding is the only approach to approve and improve it.

