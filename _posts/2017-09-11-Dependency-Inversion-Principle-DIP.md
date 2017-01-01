---
layout: post
title: Dependency Inversion Principle
tags: best-practice solid
---

In this article, we will see the Dependency Inversion Principle (DIP), which is the “D” in the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles.

> “The Dependency Inversion Principle (DIP) states that High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstraction..” — Uncle Bob Martin, Agile Software Development: Principles, Patterns, and Practices

## DIP

Dependency Inversion Principle is about two concepts: decoupling and modularization.

Let me explain more ...

When I started writing Java Swing apps in 2010 I had used to create classes and instantiate them inside the classes that used them. So they depended directly on those implementation details. 

Today we consider decoupling and polymorphism, so we use interfaces to make our dependencies clear. This is called the Dependency Injection (DI) or Inversion of Control (IoC), and it is not Dependency Inversion Principle.

Inversion of Control is a principle by which the control of objects is transferred to a container, means instead of creating an object using the new operator, let a generic container do that for you.

Dependency injection is a technique through which to implement IoC for resolving dependencies, where implementation objects are passed into another object to behave correctly.

Closely linked to the DIP are the DI and the IoC, they all work great together. So to be clear...

The Dependency Inversion Principle is all about separating your classes from concrete implementations and having them depend on abstract classes or interfaces. 

> The "lower-level" implementations are dependent on the interfaces owned by the "higher level" implementations. 

This promotes the mantra of coding to an interface rather than an implementation. By applying this, the flexibility increases in the system, and it ensures that some specific component is not tightly coupled to one implementation.

## Example

Example: Let say you want to switch on a broken door in a room. You don’t have to break the existent door frame and cause a "bordel". The work is done before, so all you have to do is to buy a door that fits in the frame because its dimensions are standard. Doors are built based on these dimensions which represent the interface. 

### Normal version

```java
class Room        
 constructor()
   door = new Door(200, 60)
 end
end
```

### DIP version

```java
class Room
 constructor(newDoor)
  door = newDoor
 end   
end
```

## Conclusion

In this tutorial, we’ve learned what is DIP and I hope you understood what DIP is and made you aware of its traits.