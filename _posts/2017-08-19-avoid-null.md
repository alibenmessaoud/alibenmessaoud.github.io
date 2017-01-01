---
layout: post
title: Avoid Null
tags: best-practice java null
---

How to prevent the famous `NullPointerExceptions` in Java? This is one of the most important questions for every Java developer to ask independently of their level of expertise. 

> I call it my billion-dollar mistake. It was the invention of the null reference in 1965 - [Tony Hoare](http://en.wikipedia.org/wiki/Tony_Hoare) 

It’s a mistake, and that’s why we should know how to deal with it, get in the habit of forbidding code from using `null`, and then prevent `NPE` at all.

Here's a Checklist of simple things to write down on a paper next to your screen:

- [ ] Avoid initializing variables to `null`
- [ ] Avoid returning `null`
- [ ] Avoid passing and receiving `null` parameters

## Avoid initializing variables to `null`

I am sure you saw code in a method starting with a `null` initialization:

```java 
public String getFormattedDescription(String id) {
  String description = null;
  if (descriptionMap.contains(id)) {
    description = descriptionMap.get(id);
  } else {
    description = "default description";
  }
  return format(description);
}
```

I think we must not declare a variable until we know what value it should hold in the next lines of code. We can get rid of this problem by adapting the structure of our code:

```java 
public String getFormattedDescription(String id) {
  String description = getRawDescription(id);
  return format(description);
}

public String getRawDescription(String id) {
  if (!descriptionMap.contains(id)) {
  	return "default description";
  }
  return descriptionMap.get(id);
}
```

With this design, we are sure that `null` can not be leaked. 

## Avoid returning `null`

Never return `null`... never return `null`... never return `null`... sing it... but we have `Optional`’s API that can make it very easy to deal with the scenario where no `Object` was produced.

## Avoid passing and receiving `null` parameters

The simple solution is to focus on never passing `null`. By doing this you will end up writing less code and avoid taking decisions about how to manage `null` inside of a method that doesn't have sufficient context to decide what to do:

````java 
public string transform(string first, 
                      string second, 
                      string third){

  // are the paramters required ?
  if(first == null && second == null && third == null) {..}
  // or one of them is required ?
  if(first == null && second == null || third == null) {..}
  // but which one is optional ?
  if(first == null || second == null && third == null) {..}
  // it is diffcult to decide! 
  if(first == null || second == null || third == null) {..}
  ...
}
````

The problem is we don't know what we should do? We have lots of choices, but none of them are explicit. It is more suitable to make sure that we never pass `null` into methods, rather than write this type full of `null` verification. Returning `null` in other methods forces us to write more code to deal with that `null` in the caller method!

## Acceptable Nulls

In a class, the value of an attribute can be `null` so the logic behind this class must be aware of that absence of the value. So the access can be secured to avoid leaking this `null` value. 

## Conclusion

You have one way to choose, be with the team of the solution to the billion-dollar problem or being part of it. Keeping this in mind will help to write code without leaking `null` and avoid `NullPointerException`.