---
layout: post
title: Understanding Java Streams Operations
tags: java java8 streams
---

In this tutorial, we'll walk through the Optional class methods that were introduced in Java 8 and improved with new features in the last versions of Java.

What is?

A Stream is a sequence of elements supporting parallel and aggregate operations. Further, Java 8 supports processing streams through operations defined in a declarative way. These operations are linked-up according to the principle of pipelines and they access the elements via Internal Iterations.

Structure of Java Stream Operations

The basics concepts of Stream API are – a source, one or more intermediate operations, and a terminal operation. These three types of components are pipelined in a sequence to make a stream work.

```json
Source -> Intermediate operations -> Terminal operation 
```

Let us now understand the three concepts of stream operations shown above –

- Source: it is the source of data from which a stream is generated. We are able to create data streams of anything because they are cheap and ubiquitous, anything can be a stream: variables, user inputs, properties, caches, data structures, etc.
- Intermediate Operation: This type of operation is invoked on a stream to process the input and return a stream instance as output. Examples of commonly used intermediate operations include `map()`, `filter()`, `limit()` and so on.
- Terminal Operation: It is responsible for giving the final output for a process and does not return a Stream. It can return any value or even no value such as in the case of `forEach()`. Common examples of terminal values are `findAny()`, `allMatch()`, `forEach()`, etc.

Lazy execution of Streams

Streams are executed in lazy way and optimize the process by taking a high level view of the entire set of pipelined operations and then applying the optimization techniques to improve efficiency of execution. Let us first take an example to understand the lazy nature of stream execution:

```java
Stream.iterate(0, n -> n + 2)
    .peek(value -> System.out.println("Value at: " + value))
    .limit(10)
    .forEach(System.out::println);
```

The output of the above code:

```sh
Value at: 0
0
Value at: 2
2
Value at: 4
4
Value at: 6
6
..
Value at: 16
16
```

In the example, `iterate()` helps to generate an infinite number of even numbers starting from the initial value provided, which is 0. Imagine if streams were not optimized, so `iterate()` would normally generate a large number of integers, which should have been passed to the intermediate operations. Finally, the stream would have been limited to just 10, and only the first 10 of this huge list of numbers should have been printed using `forEach()` method. Instead, there are multiple optimizations at play here which make the above Stream efficient –

- Firstly, streams are lazy. In other words, the actual execution does not start until the terminal operation is encountered. So, we can have `x` intermediate operations, but their execution does not start unless a terminal operation is invoked.
- Secondly, only 10 integers are generated, instead of a huge list. The nature of streams allows the implementing logic to ‘understand’ that there is a `limit(10)` in the operations, and hence, finally, only 10 elements will be used. This optimization is technically named Short-Circuiting!
- Thirdly, each element is generated, peeked into, and printed as if these three operations were lined up as individual members of a single pass instead of getting executed individually all the elements. This joining of operation is an optimization technique known as loop fusion.

Types of Intermediate Operations

Intermediate operations can be categorized into two categories –

- Stateful: These operations keep the state from a previous invocation internally to be used again in a future invocation of the method. As an example, `sorted()` requires to store elements in temporary storage as it sorts them over multiple passes.
- Stateless: These operations are the opposite of stateful and do not store any state across passes. As an example include among others `filter()`, `map()`, and `findAny()`, it also helps parallel invocations as there is no information to be shared, or any order to be maintained, between these invocations or passes.

## Conclusion

In this article, we saw what Java Streams are, understood the inner working, and saw the concepts and various terms that describe a Stream.