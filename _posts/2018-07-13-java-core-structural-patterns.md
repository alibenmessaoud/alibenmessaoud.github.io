---
layout: post
title: Java Core Structural Patterns
tags: java patterns best-practice
---

Structural patterns show how to compose classes and objects into larger structures while keeping flexibility and efficiency. We can find them in the Gang of Four book. In this post, we will see seven of them.

## Adapter 

> Allows objects with incompatible interfaces to collaborate.

It is useful to make an application work with a legacy class whose source code cannot be modified, a 3rd-party class, or any other class with a weird interface.

Methods takes an instance of different abstract/interface type and returns an implementation of own/another abstract/interface type which decorates/overrides the given instance. JDK's collection framework provides Arrays class where it uses the adapter pattern:

```java
List<String> cities = Arrays.asList("Iliya", "Aelia");
InputStreamReader(InputStream) (returns a Reader);
OutputStreamWriter(OutputStream) (returns a Writer);
```

## Bridge

> Allows splitting a set of closely related classes into two separate hierarchies—which can be developed and used independently.

It attempts to solve this problem by switching from inheritance to composition. The idea is to extract one of the dimensions into a separate class hierarchy so that the original classes will reference an object of the new hierarchy, instead of having all of its state and behaviors within one class.

Methods take an instance of different abstract/interface type and return an implementation of own abstract/interface type that delegates/use the given instance.

JDBC API acts as a link between the databases such as PostgreSQL and their particular implementations. Data access code using JDBC depends only on interfaces such as `Connection`, `Statement` or `ResultSet`, instead of caring about the actual database system the application is connected to.

```java
Connection connection = DriverManager.getConnection(url);
```

## Composite

> Allows to compose objects into tree structures and then work with these structures as if they were individual objects.

Methods take an instance of the same abstract/interface type into a tree structure.

Containers objects in Swing are great examples of the composite pattern in Java. `JTabbedPane`, `JButton`, `JCheckBox`, and `JFrame` – are descendants of `Container`.

```java
JTabbedPane pane = new JTabbedPane();

pane.addTab("1", new Container());
pane.addTab("2", new JButton());
pane.addTab("3", new JCheckBox());
```

## Decorator

> Allows attaching new behaviors to objects by placing these objects inside particular wrapper objects that contain the behaviors.

It enhances the behavior of an object without modifying the original object itself.

In the next example, `BufferedInputStream` is decorating the `FileInputStream` to add the capability to buffer the input.

```java
BufferedInputStream binputStream = new BufferedInputStream(
    new FileInputStream(new File("file.txt"))
);

while (binputStream.available() > 0) {
    char c = (char) binputStream.read();
    System.out.println("Char: " + c);
}
```

## Façade

> Provides a simplified interface to a library, a framework, or any other complex set of classes.

It means shields the user from the complex details of the system and provides them with a `simplified view` of it, which is `easy to use` where methods internally use instances of different independent abstract/interface types.

When you call a shop to place a phone order, an operator is your façade to all the shop's services and departments. 

[`javax.faces.context.ExternalContext`](https://docs.oracle.com/javaee/7/api/javax/faces/context/ExternalContext.html), which internally uses [`ServletContext`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html), [`HttpSession`](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpSession.html), [`HttpServletRequest`](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html), [`HttpServletResponse`](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletResponse.html), etc.

## Flyweight

> Allows fitting objects into the available amount of RAM by sharing common parts of the state between multiple objects instead of keeping all of the data in each object.

In other words, if we have immutable objects that can share state, as per this pattern, we can cache them to improve system performance.

As example, we list [`java.lang.Integer#valueOf(int)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#valueOf-int-) (also on [`Boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html#valueOf-boolean-), [`Byte`](https://docs.oracle.com/javase/8/docs/api/java/lang/Byte.html#valueOf-byte-), [`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#valueOf-char-), [`Short`](https://docs.oracle.com/javase/8/docs/api/java/lang/Short.html#valueOf-short-), [`Long`](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html#valueOf-long-) and [`BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html#valueOf-long-int-)).

## Proxy

> Allows providing a substitute or placeholder for another object.

Like the façade pattern, the proxied object interface is the same as that of the proxy.

A proxy instance serviced by the invocation handler we have just defined is created via a factory method call on the `java.lang.reflect.Proxy` class:

```java
Map proxyInstance = (Map) Proxy.newProxyInstance(
  DynamicProxyTest.class.getClassLoader(), 
  new Class[] { Map.class }, 
  (proxy, method, methodArgs) -> {
    ...
});
```

Once we have a proxy instance, we can invoke its interface methods as normal:

```java
proxyInstance.put("hello", "world");
```

## Conclusion

In this post, we saw practical usages of structural design patterns implemented in Java.