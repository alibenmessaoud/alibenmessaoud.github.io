---
layout: post
title: Static Factory Methods
tags: java best-practice
---

Consider static factory methods instead of constructors; this is the first rule from Effective Java. This rule claimed that developers should more often consider using static factory method instead of constructors. Java constructors are the default mechanism to initialize instances. In this post, we'll see the advantage of using static factory methods against ordinary Java constructors.

First, there's nothing wrong with constructors.

Here are some Java examples of static factory method usage:

```java
String number = String.valueOf(12);
String trueValue = String.valueOf(true);
Optiona<String> text = Optional.of("text");
Optional<String> empty = Optional.empty();
List<Integer> unmodifiableList = List.of(1, 2, 4);
List<Integer> integers = Collections.unmodifiableMap(1, 2, 4);
```

Static factory methods are a powerful alternative to constructors. Here are some of their advantages:

- Static factory methods can prevent constructors from performing other tasks than just initializing fields.
- Static factory methods provide more strict and controlled instantiation.
- Static factory methods can have a more flexible range of returning types and hide an object behind an interface.
- Static factory methods can reduce the verbosity of creating parameterized type instances.
- Static factory methods can have meaningful names conveying what they do (guess what does `3` mean in `new ArrayList(3)`).

Here are some disadvantages:

- Static factory methods cannot be used in subclasses construction because we need to use a superclass constructor. Arguably this can be a blessing in disguise, as it encourages programmers to use composition instead of inheritance.

Of course, we can implement our static factory methods.

```java
class Person {
  
  private String firstName;
  private String country;
  // etc. - more attributes  
    
  public static Person createWithFirstName(String firstName) {
    // do some stuff with input values
    return new Person(firstName, "earth", ...);
  }
  // etc. - more factory methods

  // private constructor
  private Person(String firstName, String country, ...) { }

  // useful method
  public String getDisplayName() { }
}
```

In this post, we presented the advantages of using static factory methods and show a few use cases to be a better alternative to Java default constructors.