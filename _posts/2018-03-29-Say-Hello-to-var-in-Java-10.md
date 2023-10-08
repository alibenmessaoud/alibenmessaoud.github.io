---
layout: post
title: Say Hello to var in Java 10
tags: java
---

Java 10 had been published last March, 20th 2018 and it came with new features like local-variable type inference ([JEP 286](https://openjdk.java.net/jeps/286)). This version carries the `var` keyword to declare local variables without typing with the type information: `var users = new ArrayList<User>();`

As a Java developer, we’re used to typing types twice for a long time, once for the variable declaration and once again for the constructor of the object: `URL url = new URL("https://example.io");` We also often declare types for variables that are used just once and on the next line. This is not particularly terrible, but it is somewhat redundant. From Java 10 on, `var` keyword will enable us to declare the local variable without type information supported by the type inference of the Java compiler. We can write a simpler code `String str = "Hello!"` like `var str = "Hello!"`

When JVM processes the `var` keyword, it looks at the right-hand side of the declaration and uses the token for the variable typing. And not just for internal manipulation, but it writes that resulted type into the bytecode. This saves a few characters when typing, eases lines reading, and removes duplication.

But is it practical for use? What about maintainability? won’t var hurt readability?

> ## No, This Is Not JavaScript

var will not change Java’s commitment to static typing by one iota. De-compilation of the .class file will give code without var. 

When it can be used?

The JEP 286 tells where var can be used: for local variables. More precisely, for “local variable declarations with initializers”, so even the following won’t work: `var foo; foo = "Foo";`. It really has to be `var foo = "Foo"`.  Also, none of this will work: `var ints = {0, 1, 2};` or `var appendSpace = a -> a + " ";`. The only other eligible spots besides local variables are for loops and try-with-resources blocks.

But why, if it is limited? Let’s have a look behind the scenes and find out why var was introduced.

Java’s overall trend to be pretty verbose compared to the recent languages. var was added under the project Project Amber which tries to explore and incubate smaller, productivity-oriented Java language innovations. In short, var is about decreasing verbosity and enhance reading, and it is not about saving characters but this makes the readability more painful. It needs more actions to know the type by jumping the code with IDE, and we cannot do that in GitHub.