---
layout: post
title: Why calling remove on a list throws UnsupportedOperationException
tags: java
---

You may face this `UnsupportedOperationException` when playing with some Java collections from various sources or creation operations.

The root of this exception is that the returned object has a fixed-size and maybe uses one object of `java.util.Arrays`. 

All the methods provided by the `Arrays` class are static in nature and some of its methods throw the `UnsupportedOperationException`. `add` or `remove` methods are the concerned ones for this type of fixed-size lists.

This is different from what we expected to return, which is the standard `java.util.ArrayList`.

The solution is to use `java.util.ArrayList` constructor to create a new list from an object based on `java.util.Arrays`:

```java
ArrayList<String> newList = ...
// if it throws UnsupportedOperationException
newList = new ArrayList<>(badArrayList);

// if you used `asList` to create a list
ArrayList<String> list = Arrays.asList("a", "b", "c");
//...
// create a new list with ArrayList and adapt your code
ArrayList<String> newList = new ArrayList<>(list);
```

In conclusion, it's essential to understand that removing or adding objects of a list can be problematic and how to find a simple and efficient solution.

