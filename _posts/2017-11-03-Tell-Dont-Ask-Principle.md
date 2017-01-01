---
layout: post
title: Tell Don’t Ask Principle
tags: best-practice
---

Whenever we create a new application, designing classes is among the first and important tasks because the taken decisions at that phase will and could affect the rest of our work. So to succeed this step, we should apply the best practices and principles. Especially when defining the boundaries and responsibilities of each class as we are doing object-oriented programming (OOP) which is unlike the procedural programming (PP).

>  Procedural code gets information then makes decisions. Object-oriented code tells objects to do things. — [Alec Sharp](https://twitter.com/alecsharp)

I will not talk about PP but I just want to clarify that OOP and PP are two different programming paradigms. For some applications or domains, OOP is suitable and it is not better than PP, and vice versa. In the past when I started learning computer and programming and algorithm I used PP languages like Pascal and C, then I learned Java and OOP. To be honest and as I remember this change was not easy. OOP first requires a mind-shift, and here it comes **Tell Don’t Ask** principle. This principle focuses on the design of classes particularly their behavior, and by hiding their internal working using encapsulation techniques.

To understand the tell and the ask, let's write a code to show them separately. These examples are about a loyalty card system that removes and adds points to a loyalty card class.

## 1. Ask version

Let's define the class `LoyaltyCard `: 

```java
class LoyaltyCard {
    private int id;
    private int balance;
    
    // getter, constructor removed for brevity's sake
}
```

To manage the points, we wrote a class to add and remove points from a `LoyaltyCard ` object:

```java
class LoyaltyCardService {
    // constructor removed for brevity's sake
    
    void addPoints(int id, int points) {
        LoyaltyCard card = //... find card by id in db
        card.balance += points;
        // persist the LoyaltyCard
    }
    
    void removePoints(int id, int points) {
        LoyaltyCard card = //... find card by id in db
        if (card.balance < points)
            throw new Exception("Not enough points.");
        
        card.balance -= points;
        // persist the LoyaltyCard
    }
}
```

*Can you spot bad design when you see it? Can you tell what’s wrong down ?* 

In this version, `LoyaltyCard` is a data holder and does not have any logic. Moreover, `LoyaltyCardService` does all the work and it always asks the `LoyaltyCard` the balance to add or remove points. `LoyaltyCardService` has a lot of responsibilities that shouldn't have. 

So what will be the design if the business asked to have `Standard`, `Premium`, and `Gold` Loyalty Cards? For example, if the card is `Premium`, we need to add an extra point to the balance and two extra points if it is Gold: So we'll have to modify the `LoyaltyCard`  and `LoyaltyCardService` classes and we will end up with messy code and logic of `LoyaltyCard` inside `LoyaltyCardService`. At a moment, we will not know who's responsible for what. 

The answer is to make `LoyaltyCardService` do simple operations add/remove points and nothing else.

## 2. Tell version

Let's add the card type in the `LoyaltyCard` class:

```java
class LoyaltyCard {
    private int id;
    private int balance;
    private int cardType;
    
    // getter, constructor removed for brevity's sake
    
     void addPoints(int points) {
         int extraPoints = 0;
         if (cardType == 1) extraPoints = 1;
         if (cardType == 2) extraPoints = 2;
         balance = balance + points + extraPoints;
    }
    
    void removePoints(int id, int points) {
        if (card.balance < points)
            throw new Exception("Not enough points.");
        card.balance -= points;
    }
}
```

In this version, the `LoyaltyCardService` focuses on its responsibility and it has no logic. 

```java
class LoyaltyCardService {
    // constructor removed for brevity's sake
    
    void addPoints(int id, int points) {
        LoyaltyCard card = //... find card by id in db
        card.addPoints(points);
        // persist the LoyaltyCard
    }
    
    void removePoints(int id, int points) {
        LoyaltyCard card = //... find card by id in db
		card.removePoints(points)
       	// persist the LoyaltyCard
    }
}
```

All the logic of adding or removing points, extra points, and card type is now in the `LoyaltyCard` class, where it should be. Moreover, to protect the balance inside the `LoyaltyCard` class against unintentional uses, only the `LoyaltyCard` instance can update its balance.

Now, the logic can be extended in `LoyaltyCard` without changes in `LoyaltyCardService`.

## 3. Conclusion

In this tutorial, we have seen the Tell, Don't Ask Principle and seen bad and good examples. The Tell, Don’t Ask principle lets us concentrate on your class responsibilities and the functionalities we want them tp provide. Note, to act, we don't have to ask the objects about their state, only instruct them to.