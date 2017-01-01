---
layout: post
title: Liskov Substitution Principle
tags: best-practice solid
---

In this article, we will see the Liskov Substitution Principle (LSP), which is the “L” in the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles.

> “Subtype Requirement: Let φ(x) be a property provable about objects x of type T. Then φ(y) should be true for objects y of type S where S is a subtype of T.” — Barbara Liskov

Well, this is the most difficult definition of five SOLID principles.

Robert C. Martin has modified it to a more developer-friendly version:

> “Derived classes must be substitutable for their base classes.” — Robert C. Martin

We will explain this principle by presenting some code:

## Bad Example

Consider we have a `Bird` interface with a `fly` method:

```java
public interface Bird {
  void fly();
}
```

The duck can fly because it is a bird: 

```java
public class Duck implements Bird {
    ....
}
```

But what about Ostrich or Penguin :

```java
public class Ostrich implements Bird {
    throw new NotSupportedException("Ostrich can't fly");
}
```

This is a quite typical violation of LSP: `Ostrich` is a bird, but it can't fly. `Ostrich` class is a subtype of class `Bird`, but it can't use the fly method so we threw `NotSupportedException`. It means that we are breaking LSP principle.

> “LSP is often violated by attempts to remove features.” — Mark Seemann

If we have the `letThemFly` method to let birds fly away:

```java
public void letThemFly(List<Bird> birds) {
  ...
}
```

One (horrible) way to hide the exceptions and address this would be to change the `letThemFly` method:

```java
public void letThemFly(List<Bird> birds) {
  for(Bird bird: birds)
      if (!(bird instanceof Ostrich))
          bird.fly();
}
```

Practically what LSP indicates is that if we use superclass, it is possible to replace it with its subclass, then `letThemFly` will still run successfully.

## Good Example

The fix here is to change the interface based on what each client needs (Interface Segregation Principle, ISP).

The `Bird ` interface will be the base class/ mother of all "birds":

```java
public interface Bird {
}
```

Flying birds extend the class `Bird` :

```java
public interface FlyingBirds extends Bird {
    public void fly();
}
```

Now we can create `Ostrich` or `Penguin ` without violating the principle: 

```java
public class Duck extends FlyingBirds {}
public class Ostrich extends Bird {} 
```

And the `letThemFly` method:

```java
public void letThemFly(List<Bird> birds) {
  for(Bird bird: birds)
      bird.fly();
}
```

> OOP is not only about simple mapping real world to objects.
>
> OOP is about creating abstractions, not concepts!

## Checklist

There is a checklist to determine whether or not you are violating LSP:

- [x] Derived class throws an exception
- [x] Derived class overrides a method by an empty method
- [x] Derived class overrides a method by a new behavior
- [x] Derived class changes the unchangeable
- [x] Derived class forces to change the unchangeable
- [x] Derived class deprecate an inherited method

## Conclusion

In this tutorial, we’ve learned what is LSP and I hope you understood what LSP is and made you aware of its traits. The LSP is also, at times, termed as “Design by Contract”.