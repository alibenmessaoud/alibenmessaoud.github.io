---
layout: post
title: Returning Null vs Exception
tags: best-practice java null exception
---

When designing an API, developers may always question what to set as a returned value when an operation cannot fulfill its contract? I don't know what do you think? Should we return null? Or should we throw an exception? We write and consume APIs every day! So what do you do in this case? As a user of some API, I want to use an easy and understandable API. Also, as an author, the API must be easy to write, read, and maintain.

Hmm. Well, I like the idea of throwing an exception instead of returning null or other error values. I would argue that a null return complicates the control flow in an application as well as injects ambiguity. 

Let's get some code to show my point. We have `CoffeeMachine` interface and we need to write its implementation. 

```java
public interface CoffeeMachine {
    Coffee buy();
}
```

So we have some choices to make. Let's think about this API and how another developer can work with it? 

### Scenario #1: 

#### At the provider

In the implementation we return `null`;

▲ It is a common pattern and an easy solution. 

#### At the consumer

Let’s also look how we can consumed this API:

```java 
if (coffeeMachine.buy() != null) {
    Coffee coffee = coffeeMachine.buy();
    // do the rest of thw work
} else {
    // handle the problem.
}
```

▼ Developers need to check for `null`. 

▼ The scope of `coffee ` is the block `if`. 

▼ The `null` injects ambiguity into the calling application. 

What does it mean `null`?

Ambiguity can raise when the `buy` is method working correctly but one information is missing. Imagine, it is about a stupid thing not in the database or something technical and not functional or vice versa. Here, `null` winds up with multiple meanings, resulting in the ambiguity. 

### Scenario #2: 

#### At the provider

We decided to modify the API and add a method to check if the machine is working or not to buy coffee. The API becomes:


```java
public interface CoffeeMachine {
    Coffee buy();
    boolean isWorking();
}
```

▼ Bad design where we have two methods to describe one operation.

▼ The author needs to work more on the code.

▼ The machine can have many different states but here we can get one single state.

#### At the consumer

The consumer will need to check if the machine is working before calling the `buy` method. 

```java 
if (coffeeMachine.isWorking() != null) {
    Coffee coffee = coffeeMachine.buy();
    // do the rest of thw work
} else {
    // handle the problem.
}
```

▲ The `coffee` is guaranteed by the API inside the if block. It is better than scenario #1.

▼ The developer may find himself in scenario #1 when he'll forget to call `isWorking()`. He always needs to check if the machine is working.

▼ Developers may forget and check the `coffee` variable again.

```java 
if (coffeeMachine.isWorking() != null) {
    Coffee coffee = coffeeMachine.buy();
    if (coffee != null) {
    	// do the rest of the work
    }
} else {
    // handle the problem.
}
```

▼ Scenario #1 and scenario #2 have the same structure of code `if..else`.

This means one thing: lack of consistency throughout the application.

### Scenario #3: 

#### At the provider

We're taught that an exception is only supposed to be raised if something *unexpected* occurs. So developers will be in scenario #1 and #2 all the time.

So we decided to throw an exception `MachineExcption` in the implementation.

```java
public interface CoffeeMachine {
    Coffee buy() throws MachineExcption;
}
```

▲ API is concise.

In other words, the API must throw an exception when there's no `coffee` which means the contract is not respected. 

#### At the consumer

The solution is cleaner than the other scenari.

```java 
try {
    Coffee coffee = coffeeMachine.buy();
    // do the rest of the work
} catch (MachineExcption ex) {
    // handle the problem.
}
```

▲ The `coffee` variable will never be `null` inside the block `try`. This solution is better than in scenario #2.

 ▲ No need to check if `coffee` is `null` or not, or check if the machine is working or not.

▲ This solution is advantageous to the author and the consumer alike.

### Before You Go

- Throw an exception when methods cannot fulfill their contracts.
- Throw a checked exception there's a contract violation to be handled by the consumer.
- A method should never return `null` to represent contract failure.

### Conclusion

`Null` values make programming more difficult. Throwing an exception has many benefits: Cleaner control flow in your calling code; Removes ambiguity of what "null" means; Improved consistency between method behavior in an application.