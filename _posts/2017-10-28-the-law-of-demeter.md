---
layout: post
title: The Law of Demeter
tags: best-practice
---

The "Law" of Demeter (LoD) is a design guideline for developing object-oriented software. In its general form, the Law of Demeter is a specific case of loose coupling.

>  Good men must not obey the laws too well. — Ralph Waldo Emerson

Let’s take a deep dive and see what it means, and how to obey it?

## 1. The LoD

The Law, in its original form, is stated this way:

> For all classes C, and for all methods M attached to C, all objects to which M sends a message must be:
>
> - M’s argument objects, including the self-object or
> - The instance variable objects of C
> 
> (Object created by M, or by functions or methods which M calls, and objects in global variables are considered as arguments of M.)

There's another simple version, it states that an object should talk to “friends” and not “strangers”. 

In other words, a part of the code of an object must call:

1. any method in its own class, or
2. any object that is an attribute of this object, or
3. any object that was passed as a method’s parameter or
4. any object that was locally created

## 2. Why Should We Obey the Law of Demeter?

This enforces hiding the internal structure of the objects, reduce dependencies, and build a codebase resistant to change. Automatically, it guarantees proper encapsulation, cohesion, and loose coupling.

## 3. Code

Here's a simple example for the 4 rules:

```java
public class DBConnection {
    private static int MAX_CONNECTION = 1;
    private DbConnectionFactory factory;
    
    public void close() {
        ...
        clear(); // Allowed Rule #1
        ...
    }
    
    public Connection open() {
        ...
        factory.createAndOpen(); // Allowed Rule #2
        ...
    }
    
    public Connection open(Pool pool) {
        ...
        pool.create(); // Allowed Rule #3
        pool.setMaxConnections(MAX_CONNECTION); // Allowed Rule #2
        ...
    }
    
    public Connection open() {
        ...
        Connection c = create(config);
        c.open(); // Allowed Rule #4
        ...
    }
    
    public Connection open() {
        factory.getDefaultConfig().getHost() // Not Allowed!!
    }
}
```

The next chain calls violates the law: 

```java

car.getOwner().getAddress().getStreet();
// or
Owner owner = car.getOwner();
Address ownerAddress = owner.getAddress();
Street ownerStreet = ownerAddress.getStreet();
```

As above, the objects in `owner`, `ownerAddress`, and `ownerStreet` are still not covered by any of the rules, therefore no methods should be called on them.

Fluent APIs are also forbidden! But ..

```java
APIClient client = anAPIClientBuilder()
  .withHost("..") // Rule #4
  .withAppKey("..") // Rule #4
  .withSecretKey("..") // Rule #4
  .build();
```

`APIClientBuilder` object is created in this method right now, so the first method call `withHost(...)` is on a freshly created object. This is explicitly allowed by Rule #4.

“Wrapper” methods is a way to hide violations under the carpet:

```java
class Garage {
    Plane plane;
    
    public void leaveGarage() {
        ...
        plane.isPilotAllowedToFly(); // Not Allowed!! ... because
        ...
    }
}

class Plane {
    private Crew crew;
    
    public boolean isPilotAllowedToFly() {
        ...
        return crew.getPilot().isAllowedToFly(); // Not Allowed!!
    }
    ...
}

//or
plane.getCrew().getPilot().isAllowedToFly(); // Not Allowed!!
```

According to Robert C. Martin, the Law could be circumvented if we use direct access to fields:

`plane.crew.pilot.allowedToFly; ` // Allowed but.. 

Robert C. Martin stated that direct access to fields arguably still qualifies as “sending a message,” since it is still just communication between two objects. Moreover, the Law of Demeter should not be applied to pure data structures.

> Pure data structures (no behavior, just data) are integral building blocks of a procedural, functional, or a mixed paradigm approach. 

In the end, remember this law is a guideline to develop good object-oriented software, and there are cases when they don’t apply.

## 4. Conclusion

In this tutorial, we have seen the Law of Demeter by definition and seen bad and good examples.