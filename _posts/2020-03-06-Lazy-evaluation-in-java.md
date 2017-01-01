---
layout: post
title: Lazy evaluation in Java
tags: java
---

Lazy evaluation is a strategy that delays the evaluation of an expression until its value is asked. The reverse is eager. Programming languages are classed into strict and lazy. As an example, Java is a strict language, and Haskell is lazy by default.

In most cases, Java eagerly evaluates an expression bound to a variable: when we write `x = 5`, the instruction makes the literal value `5` to be referenced by the variable named `x`. However, if we write down `x = 2 + 3`, `x` will reference the operation's result and not `2 + 3`.

Java supports lazy evaluation for the following specific syntax:

- The Boolean `&&` and `||` operators, which will not evaluate their right operand when the left operand is false (`&&`) or true (`||`) [the minimal evaluation or short-circuit].
- The `?:` operator, which evaluates a Boolean expression and subsequently evaluates only one of two alternative expressions (of compatible type) based on the Boolean expression's true/false value.

In the next example, the `if..else` is considered lazy as it evaluates only the required branch and not both. But it depends on the code you write and how to achieve laziness. The first or second branch of the `if..else` code block can be evaluated, resulting in the weekend or default message to be printed:

```java
if (day != 6 && day != 7) {
  System.out.println(weekendMessage);
} else {
  System.out.println(defaultMessage);
}
```

Another case when performance can be better in line #1 and not in #2. In other words, `id` can be null so it will cause the evaluation of the expression in the `filter` with a `null` value and when the list is empty or there are no elements, the expression will not be evaluated:

```java 
public Info getById(String id) {
  return list
   .stream()
   // .filter(e -> id.equals(e.id)) #1
   // .filter(e -> e.id.equals(id)) #2
   .findFirst()
   .get();
}
```

Let's look at the following code: 

```java
String eagerMatch(boolean b1, 
                  boolean b2) {

  return b1 && b2 
      ? "match" 
      : "incompatible!";
}

boolean compute(String str) {
  System.out.println("executing...");
  return str.contains("a");
}

public static void main(String [] args) {
  String rs = eagerMatch(
      compute("bb"), 
      compute("aa"));
  System.out.print(re);
}
```

Running this program produces the following output: 

`executing... executing... incompatible! `

So, it's clear that besides the fact that the language can be lazy or strict, we may still write lazy or eager code without paying attention. 

However, we can achieve laziness by using techniques offered by the languages themselves. We can list the Supplier interface in Java, the call-by-name syntax in Scala or Guava. These libraries provide a beneficial way to create lazy-evaluated values.

From the Java specification, the Supplier interface:

- is a functional interface with a functional method called get.
- represents a supplier of results.
- does not require a new or distinct result to be returned each time the supplier is invoked.

Let's see how it works:

```java
Integer value1 = 20; // eager
Supplier<Integer> value2 = () -> 20; // lazy

Integer value3 = process(); //eager
Supplier<Integer> value4 = () -> process(); // lazy

System.out.println(value2);
```

Printing the value of v2 or v4 will as follows `Main$$Lambda$14/0x0000000800ba4840@50040f0c` 

This model is applied quite often since Java 8. One of the most obvious examples would be `Optional` with the variant `orElseGet`.

Now, whenever we want to lazily evaluate a computation returning `T`, we need to wrap the code in a `Supplier<T>` instance. 

Let’s implement a lazy version using the `Supplier ` interface of the `eagerMatch` version:

```java
String lazyMatch(Supplier<Bolean> a, 
                 Supplier<Bolean> b) {
  return a.get() && b.get() 
      ? "match" 
      : "incompatible!";
}

public static void main(String [] as) {
  System.out.println(lazyMatch(
      () -> compute("bb"),
      () -> compute("aa")));
} 
```

The output of running this program for no match: `executing... incompatible!`

The combination of lazy argument and operand evaluation enables this program to bypass the expensive execution in `compute("aa")`.l

Two important things to notice in running this example:

- compute is executed when the functional method get is invoked.
- `&&` operator exhibits "short-circuiting", which means that the second operand is evaluated only if needed.

We have seen how laziness is applied in programming languages:

- Strict languages evaluate instructions when they are declared; lazy languages do it when they are used.
- Converting an eagerly evaluated method into a lazy evaluated one using the Supplier interface in Java.
- All programming languages are lazy in some way because some portions of programs are not evaluated, depending on some conditions.

However, do not use lazy strategy everywhere but use it for cases where there are clear signs of performance improvements. These strategies can be used to do Lazy loading and caching. Stay tuned for the next article to know more about it. 