---
layout: post
title: Java Records and the rest
tags: java patterns 
---

In the next release, LTS of Java, Java 17, some of the upcoming changes and preview features like `Record`s will be released. This article focuses on `Record` type and how it can be used. First of all `record`s are not new nor modern. They were present in COBOL in the 60s and used in Pascal, C, and others.

But before going deeper, let's look at some basics of Java, the class. A classic Java class has this form:

```java
public class Car {
    private String name;
    private String brand;
    private int price;
    // methods
}
```

The `Car` attributes are necessary to define a class. Of course, we will need more things like one or multiple constructors, getter and setter methods, and also `equals()`, `hashCode()` and `toString()`.

> Some of us use some libs like `Lombok` to generate these methods and reduce the amount of boilerplate code but they also have some drawbacks.

For a long time, we needed a Java mechanism to write and use data objects and "modeling data as data".

`Lombok` annotations provide the tools to define immutable data objects:

```java
@Getter
@RequiredArgsConstructor
@EqualsAndHashCode
@ToString
public class Car {
    private String name;
    private String brand;
    private int price;
}
```

And here come the day we see the previous code written using the keyword `Record`. So our `Car` class definition becomes:

```java
public record Car(
   String name,
   String brand,
   int price
){}
```

We need to specify the record type name right after the `record` keyword and then the attributes.

The main reason given in the proposal to have records is for “modeling data as data“: A Record in Java is a special form of a class that only holds this data.

What does a record offer us? Once declared, we get a class that has an implicit constructor accepting all the values of the components of the Record. We automatically get implementations for the `equals()`, `hashCode()` and `toString()` methods based on all the components in the record. Additionally, we also get accessor methods for each component we have in the Records.

Bye, bye Lombok. 

Also, records are always immutable. There are no setter methods. Once a record is instantiated, you can no longer modify it.

Again, bye-bye Lombok.

Also, the record classes are final. You can implement an interface with a record, but you cannot extend from any other class when defining a record. Overall, there are a few restrictions here. But records give us a very powerful way to concisely define data-only classes in our applications.

Really, bye-bye lombok?

No. 

Compared to Record, Lombok just generates code and we have all the freedom you need to adapt the class to our requirements.

How to Think about records?

A `record` is a new, restricted form of a class used to model data as data. It is not possible to add additional state such as non-static fields in addition to record attributes. Records are really about modeling immutable data. Consider it as tuples, but not just tuples where we have index-based references in some other languages. In Java, tuple elements have actual names because names matter in Java.

How not to think about records?

Records are not intended to reduce boilerplate code. It is true that we have a new and concise way to define classes, but we should not fall into the trap of using them everywhere. Why? In simple words, they have limitations.

> A JavaBean is a Java object that satisfies certain programming conventions:
>
> 1. The JavaBean class must implement either `Serializable` or `Externalizable`
> 2. The JavaBean class must have a no-arg constructor
> 3. All JavaBean properties must have public setter and getter methods
> 4. All JavaBean instance variables should be private

The design goal of records is to have a good way to model data as data. It's also not a direct replacement for Java beans or POJOs, because the getter methods, for example, don't follow Java Beans getter standards. Also, they are not a replacement for Java Beans in any meaningful way. Moreover, we shouldn't think of records as value types.

Where would you use records?

Records' semantics restrict what a regular class can have. We can't add hidden state and rename accessors, or change the return type and value. And fields are `final`, and can not be inherited.

But we can use it in these cases:

DTO: DTOs are by definition objects that do not need any identity or behavior. They are only used to transfer data. Records will also be very useful when we want the keys of a map to consist of several values which act as a composite key. Using records in this scenario will be very useful since we automatically get the correct behavior for `equals` and `hashcode` implementations.

Read-only view in queries: records should not be used as much as possible when it comes to the Java Persistence API. Representing entities as a record is not really possible because entities are heavily based on the Java Beans convention. Of course, there might be some opportunities when instantiating objects in read-only view in queries where you might use records instead of regular classes.

Value Object: Value Object is a DDD concept that is immutable and doesn’t have its own identity. They are the backbone of any rich domain model. Records can be fit for DDD value objects — they provide the same semantics and do all the hard lifting behind the scenes.

So, while “records” seem to be universally welcomed, developers should be extra careful when considering this new feature.
