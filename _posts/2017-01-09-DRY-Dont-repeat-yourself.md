---
layout: post
title: DRY
tags: best-practice java
---

DRY, Don't Repeat Yourself, is perhaps one of the most fundamental of all the programming principles that had been introduced by Andy Hunt and Dave Thomas in The Pragmatic Programmer. In this article, we are going to explore DRY software design principle and its benefits.

## Code Duplication = Bugs Duplication

Every line of code in an application is a story and has a story and a context for which it had been written. So, it must be maintained or bugs will pop out of the box over the time. Apart design and wrong decisions, duplication is the one of the code smells and problems in large or small systems. Duplication can needlessly bloat the codebase resulting in more complexity and makes it difficult for developers to working with the system to fully understand the entire logic. Also, it means that you need to maintain the code in several locations, the same logic or behavior, and this will oblige developers to do the same bad thing every time, and forgetting about doing a change in one these locations will cause unstable behaviors, bugs and customer concerns.

## The DRY Principle: Don't Repeat Yourself

DRY or Don't Repeat Yourself, is a basic principle of software development aimed at reducing repetition of information. The principle is stated as, "*Every piece of knowledge or logic must have a single, unambiguous representation within a system*." So, learning to recognize duplication, and understanding how to eliminate it from a piece of software can help to produce cleaner code.

## The Idea

Many processes in software development are repetitive and easily automated. DRY applies in these contexts, as well as in the source code of the application. The goal is to ensure that there is only one way of accomplishing tasks, and it is as painless as possible. 

## How to Achieve DRY

Repetition in the code and logic can take many forms. To avoid violating DRY, think to divide the system into small pieces and using design patterns to reduce the code and having reusable units. Also, many design patterns have the explicit goal of reducing duplication in logic within an application. For example, Abstract Factory or Strategy patterns help to create objects and eliminate large if-then structures. There's other patterns and techniques to show but these examples are a good list to start.

## Example

If you do not apply DRY, the code will be as follows:

```java
public class Calculator {
	public int sum(int a, int b) {
		return a + b;
	}
 
	public double avg(int a, int b) {
		int sum = a + b;
		return sum/2;
	}
}
```

Now, if we want to print the sum in each method, we have to revise the methods as follows:

```java
public class Calculator {
	public int sum(int a, int b) {
        int sum = a + b;
        System.out.println("Sun of all =" + sum);
		return sum;
	}
 
	public double avg(int a, int b) {
		int sum = a + b;
        System.out.println("Sun of all =" + sum);
		return sum/2;
	}
}
```

To overcome this, we will apply the DRY principle as follows:

```java
public class Calculator {
	public int sum(int a, int b) {
        int sum = a + b;
        System.out.println("Sun of all =" + sum);
		return sum;
	}
 
	public double avg(int a, int b) {
		int sum = sum(a, b);
		return sum/2;
	}
}
```

## DRY Benefits

Less code helps to saves time, easy maintenance, and also less bugs.

## Conclusion

Keeping this software design principles in mind and using it wisely can help to save time, make the software more robust and easy to maintain.

