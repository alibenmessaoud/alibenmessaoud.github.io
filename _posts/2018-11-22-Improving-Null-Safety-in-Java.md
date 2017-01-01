---
layout: post
title: Improving Null Safety in Java
tags: java java8 optional null
---

In this article, we will present a couple of ways to prevent writing needless `null` checking by using `Optional` API and lambda expressions.

Let's consider this hierarchy of classes:

```java 
class Level1 {
  Level2 level2;
  // getter
}
class Level2 {
  Level3 level3;
  // getter
}
class Level3 {
  String name;
  // getter
}
```

Dealing with the name attribute will complex a little bit our code in the next lines if one of the parent objects is `null` and causing `NullPointerException`:

```java 
String format(Level1 level1) {
  String name = level1
    .getLevel2()
    .getLevel3()
    .getName();
    
  return name.toUpperCase();
}
```

So, in a first step, we decided to wrap check the `name` before calling `.getName()` method:

```java 
String format(Level1 level1) {
  String name = level1
    .getLevel2()
    .getLevel3()
    .getName();

  if (name != null)
    return name.toUpperCase();
  else
    return "Default";
}
```

But this will not resolve the problem because we don't which object can or will be `null`. We need to check all the values before accessing the name:

```java 
String format(Level1 level1) {
  if (level1 != null 
      && level1.getLevel2() != null 
      && level1.getLevel2()
               .getLevel3() != null) {

  String name = level1
      .getLevel2()
      .getLevel3()
      .getName();
      
  if (name != null)
    return name.toUpperCase();
  else
    return "Default";
}
```

In this first version, we managed those `null` checks and the code performed everything requested but we can not cover all the cases. So we can get rid of all those checks and verbose code by using the Java `Optional` API. 

The `Optional` class enables us to pipe multiple `map` operations in a row. Null checks are automatically handled under the hood in a declarative style:

```java 
String format(Level1 level1) {
  return Optional.ofNullable(level1)
    .map(Level1::getLevel2)
    .map(Level2::getLevel3)
    .map(Level3::getName)
    .map(s -> s.toUpperCase())
    .orElse("Default");
}
```

With this declarative style, the code describes the "what" to do and not "how" to do hence the brevity of the code.

## Conclusion

Both solutions are good and have advantages and disadvantages like performance, readability and maintainability.