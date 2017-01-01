---
layout: post
title: Java Concurrency
---

Concurrency means multiple computations are happening at the same time. It enable a software to achieve high performance by utilizing the maximum of hardware and software resources. In Java, the backbone of concurrency is threads. Within a Java application we can work with many threads to achieve concurrency. In this article, we’ll see Concurrency in Java.

## Processes vs. threads

A process runs isolated and independently of other processes. It can not access shared data in other processes. The OS gives it access to memory and CPU time.

A thread is another type of a process. It has its own call stack, but it has a private memory and can access shared data of other threads in the same process.

## Concurrency issues

There are two basic problems in the concurrency, visibility and access problems:

- visibility: A reads shared data which is later changed by B and A is unaware of this change
- access: problem can occur if several programs access and change the same shared data at the same time

Visibility and access problem can lead to:

- Liveness failure: The program does not react anymore due to problems in the concurrent access of data, e.g. deadlocks
- Safety failure: The program creates incorrect data

## Concurrency in Java

The [Concurrency API](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html) was first presented with the release of Java 5 and then progressively improved after each new Java release. A Java program runs in its process and by default in one thread. Java supports threads as part of the Java language via the `Thread` class. 

*Locks* provides another way to secure certain parts of the code to be executed by several threads at the same time. The easiest approach to lock a certain method or Java class is to define the method or class with the `synchronized` keyword.

*Volatile* keyword guarantees that any thread that reads the field will see the most recently written value. This keyword will not perform any mutual exclusive lock on the variable.

## The Java memory model

The Java memory model describes the communication between the memory of the threads and the main memory of the application. It guarantees that each thread entering a synchronized block of code sees the effects of all previous modifications that were guarded by the same lock.

Between the read and the write the value of the operation `i++` might have changed. Since Java 1.5 the java language provides atomic variables, e.g. `AtomicInteger` which provide methods like `getAndDecrement()` which are atomic.

## Immutability

The simplest way to avoid problems with concurrency is to share only immutable data between threads. Immutable data is data which cannot be changed.

## Defensive Copies

Protect a class from the calling code to change the values in the attributes. The idea is to copy data in the parameters and only return copies of data to calling code.





























































































The *synchronized* keyword in Java ensures:

- that only a single thread can execute a block of code at the same time
- that each thread entering a synchronized block of code sees the effects of all previous modifications that were guarded by the same lock

Synchronization is necessary for mutually exclusive access to blocks of and for reliable communication between threads.

You can use the *synchronized* keyword for the definition of a method. This would ensure that only one thread can enter this method at the same time. Another thread that is calling this method would wait until the first thread leaves this method.

```java
public synchronized void critial() {
    // some thread critical stuff
    // here
}
```

You can also use the `synchronized` keyword to protect blocks of code within a method. This block is guarded by a key, which can be either a string or an object. This key is called the *lock*.

All code which is protected by the same lock can only be executed by one thread at the same time.

For example the following data structure will ensure that only one thread can access the inner block of the `add()` and `next()` methods.

```java
public void add(String site) {
    synchronized (this) {
        if (!crawledSites.contains(site)) {
            linkedSites.add(site);
        }
    }
}
```













A Java application runs by default in one process. Within a Java application you work with several threads to achieve parallel processing or asynchronous behavior.