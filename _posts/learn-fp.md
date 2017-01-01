---
layout: post
title: The Basics of Elasticsearch Scroll API
tags: java elasticsearch
---

 



https://dev.to/mare_milenkovic/how-do-i-become-proficient-with-functional-programming-in-java-52pa



### My experience with Functional Programming

To get familiar with FP, I studied and applied in practice FP concepts. The most important concepts that I learned are:

- **Immutability** - I have as many as possible immutable objects in my codebase. This leads to fewer places where I can change the state of the program. And that leads to fewer bugs.
- **Referential transparency** - I write as much as possible pure functions. Those functions are like mathematical functions. For the same input, they always have the same output.
- **Pure and unpure functions** - before FP, I was not aware of this concept. Now, I separate pure functions from unpure. It allowed me to easier test the code and improved my code reusability.
- **Function Composition** - promotes better code readability and it is easier to write code by composing functions.
- **Curried Functions** - brilliant concept, but it is not natural to use it in Java like in other FP languages. I don’t use them for now.
- **Lazy evaluation** - evaluate values when they are needed. Lambdas are the way to do a lazy evaluation in Java.
- **Higher-Order functions** - receive other functions as parameters in existing functions. Those functions are usually utility functions.
- **Map, filter, reduce pattern** - Java Stream API implements this pattern.
- **Monads** - helped me understand how to handle unpure functions safely.
- **Optional class** - FP provides an efficient solution on how to work with nullable objects. I always return the Optional object instead of null in a method that can return null.
- **Railway programming** - helped me understand how Stream API and Optional class works.

Using FP in code doesn’t prevent us, developers, from writing bad code. We still need to write unit tests, have a good understanding of our task and our codebase. We still need to apply all the best practices that we learned in the past. FP is promoting good practices and makes it easier for us developers to write good and maintainable code.