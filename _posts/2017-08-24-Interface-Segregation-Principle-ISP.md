---
layout: post
title: Interface Segregation Principle
tags: best-practice solid 
---

In this article, we will see the Interface Segregation Principle (ISP), which is the “I” in the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles.

> “Clients should not be forced to depend upon interfaces that they do not use.” — Robert Martin, paper “[The Interface Segregation Principle](https://web.archive.org/web/20150905081110/https://www.objectmentor.com/resources/articles/isp.pdf)”. 

## How to get in trouble?

An Interface is a set of abstractions to be implemented by a class. In other words, we describe the behavior but don’t implement it.

We could think about our features and expose all the functionalities we want it to offer. Let's consider this interface:

```java
interface CarService {
    void drive(int speed);
    void driveAndPlayRadio(int speed, int frequency);
    void gps(int lg, int lt);
}
```

A person can drive the car, or also he can drive the car and play the radio at the same time. Notice we decided to put all the described features in a single interface `CarService `.

In real life, manufacturers can produce cars without a radio station or GPS. In our code, we are obliged to throw an exception in the method for each specific type of a car:

```java
class SimpleCarService implements CarService {
    @Override
    public void drive(int speed) {
        System.out.println("Drive at speed of " + speed);
    }

    @Override
    public void driveAndPlayRadio(int speed, int frequency) {
        throw new UnsupportedOperationException("No drive and radio 
                                                in the car");
    }
    
    @Override
    public void gps(int lg, int lt) {
        throw new UnsupportedOperationException("No gps in the car");
    }
}
```

Likewise, for a drive and play radio only car, we’d also need to throw an exception in `drive(int speed)` and `gps(int lg, int lt)` methods.

As a design, this is horrible and adds undesired side effects whenever we want to perform changes to our abstraction. I am sure that you don't want to update all the implementations because you added the mileage in the `speed` method! Also, the speed gets used twice and we can smell the problems. Also, notice that we can have the same objects `SimpleCarService ` and `CarWithRadioService ` if  we create `CarWithRadioService` and pass 0 as the frequency.

**Solution:** Respect Interface Segregation Principle!

## The principal

The purpose of ISP is to reduce the side effects of using large interfaces. Hence, it is better to break large interfaces into smaller ones. 

## Consequences of fat interfaces

By using fat interfaces, we will encounter the next problems:

- clients will be confused by the methods they don’t need
- Maintenance becomes more difficult because of side effects: update all the implementers even for unneeded features
- violation of the Single Responsibility Principle (SRP)

## Refactoring

We can refactor the `CarService` and have three separate interfaces for `DriveService`, `RadioService`, and `GpsService`. 

```java
interface DriveService {
    void drive(int speed);
}

interface RadioService {
    void driveAndPlayRadio(int frequency);
}

interface GpsService {
    void gps(int lg, int lt);
}
```

Congratulations, you’ve segregated the interfaces, so now we have features that are independent of each other.

This design provides more flexibility for the clients to use the abstractions as they may see fit and to provide implementations without additional cargo:

```java
class SimpleCarService implements DriveService, GpsService {
    @Override
    public void drive(int speed) {
        System.out.println("Drive at speed of " + speed);
    }
    
    @Override
    public void gps(int lg, int lt) {
        System.out.println("Location: " + lg + ", " + lt);
    }
}
```

As we can observe, the interfaces don't violate the principle. The implementations don't have to provide empty methods. This keeps the code clean and reduces the chance of undesired changes or bugs.

## Checklist

There is a checklist to determine whether or not you are violating ISP:

- [x] Fat interface
- [x] Unused dependencies
- [x] Methods throwing exceptions

## Conclusion

Interface Segregation Principle guides us to respect our clients more than we thought necessary.