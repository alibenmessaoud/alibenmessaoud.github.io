---
layout: post
title: Java Optional Recipes
tags: java java8 monad optional
---

In this tutorial, we'll walk through the Optional class methods that was introduced in Java 8 and improved with new features in the last versions of Java.

## About 

The optional class is a type of container for a value that may be absent.

## Origin

`NullPointerException`, `NullPointerException`, `NullPointerException`... The code below will throw the `NullPointerException` at runtime if there's no null checking before using the `employee`.

```java
Employee employee = findEmployee("1234");
System.out.println("Employee's Name = " + employee.getName());
```

Also, there's no difference between null of an `absent` value and `null` value resulted after a method call. There is no point to return `null` values if there is nothing to output. 

In both cases, it should be a value type that represents the absence or not of a returned value. Therefore, in Java 8, a new type was added called `Optional`, which indicates the presence or absence of a value. 

`Optional` has not been invented to get rid of the `NullPointerException`. But, its principal aim is to deal gracefully with `null` values in streams.

> Optional is mainly used as a method return type when it is clearly necessary to indicate "no result" and the use of null may cause an error. Variables of type Optional must never be null. An Optional should always carry to an Optional instance.

Now, Oracle is advising to use `Optional` to get rid of the `if` statement.

## When and how?

- Use massively in streams, filter because `filter()` copes with `null` values
- Primitive data types can't be null, don't do that
- `Optional` method parameters are a code smell
- Wrap returned values with `Optional`

## Create

There are diverse ways of creating `Optional` objects. 

### Do

The easiest is *empty()*:

```java
Optional<String> empty = Optional.empty();
```

We can likewise create an `Optional` object with the method `of()`:

```java
String name = "Java";
Optional<String> maybeName = Optional.of(name);
```

In case we expect any `null` value, we can use the method `ofNullable()`:

```java
String name = ...;
Optional<String> maybeName = Optional.ofNullable(name);
```

By using `ofNullable()`, a `null` reference will not cause an exception but it will return an empty `Optional` object.

### Don't

The argument passed to the method `of()` can't be `null`. Unless we'll get a `NullPointerException`:

```java
String maybeName = Optional.of(null);
```

### Note

- `Optional` is a container that may hold a value, and it is useless to initialize it with `null`.

## Get The Value

The `get()` method is simply used to return a value from the `Optional`. 

### Don't

Suppose the value is not present, then it throws the exception [NoSuchElementException](https://docs.oracle.com/javase/8/docs/api/java/util/NoSuchElementException.html).

```java
Optional<Employee> maybeEmployee = repository.getEmployeeById(1234);
Employee employee = maybeEmployee.get();
```

### Do

So it is recommended to first check if the value is present or not before calling `get()`.

```java
Optional<Employee> maybeEmployee = repository.getEmployeeById(1234);
if (maybeEmployee.isPresent()) {
    Employee employee = maybeEmployee.get();
    ... // do something with "employee"
} else {
    ... // do something that doesn't call employee.get()
}
```

### Note

- The code above is wordy and is not preferable as it looks like the null checking style.

## Get The Value Or Else

The method `orElse()` is used to return a default value if the object is empty.

### Don't

```java
Optional<String> maybeEmployeeStatus = repository.getEmployeeById(1234);
if (maybeEmployee.isPresent()) {
    Employee employee = maybeEmployee.get();
    ... // do something with "employee"
} else {
    ... // do something else like
    Employee employee = Employee.of("0", "Unknown");
}
```

### Do

```java
Optional<Employee> maybeEmployee = repository.getEmployeeById(1234);
Employee employee = maybeEmployee.orElse(new Employee("0", "Unknown"));
```

### Note

- Generally, you should avoid using `orElse(null);`
- The value returned by `orElse()` is always evaluated regardless of the optional value presence. Hence the rule is to apply `orElse()` when you have already a preconstructed object without expensive calculations.

## Get The Value Or ElseGet

The method `orElseGet()` method accepts a [Supplier](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html) and it is invoked when Optional is empty. 

### Don't 

When the employee will be returned from the cache, the database query is still called too. It is very expensive as an operation!

```java
Optional<Employee> getFromCache(int id) {
    ...
}

Optional<Employee> getFromDB(int id) {
    ...
}

public Employee findEmployee(int id) {        
    return getFromCache(id).orElse(
        getFromDB(id).orElseThrow(
            () -> new NotFoundException("Employee not found with id" + id)
        )
    );
}
```

### Do

By using `orElseGet()`, you will get a performance improvement:

```java
public Employee findEmployee(int id) {        
    return getFromCache(id).orElseGet(
        () -> getFromDB(id).orElseThrow(
            () -> new NotFoundException("Employee not found with id" + id);    
        )
    );
}
```

### Note

- Don’t even think of using `isPresent()` and `get()` pairs because they are not elegant.

## Get The Value Or Throw An Exception

There are situations when you need to throw an exception to show a value doesn’t exist. 

### Don't

```java
Employee maybeEmployee = repository.findById(id);
if (maybeEmployee.isPresent())
    return maybeEmployee.get();
else
    throw new NoSuchElementException();
```

### Do

```java
repository.findById(id).orElseThrow();

repository.findById(id)
    .orElseThrow(() -> new EmployeeException("Employee not found with id " + id));
```

### Note

- Generally, it is better to avoid using `orElse(null)`, but in such a situation, using `orElse(null)` is preferable.

## Get The Value If Present Only

Sometimes, we need to act only on the wrapped value when an `Optional` value is present.

### Don't

```java
Optional<String> maybeName = Optional.of("Java");
if (maybeName.isPresent())
	System.out.println(maybeName.get().length());
```

### Do

```java
Optional<String> maybeName = Optional.of("Java");
maybeName.ifPresent(s -> System.out.println(s.length()))
```

### Note

- The `ifPresent()` method returns nothing.

## Get The Value  For  Empty-based Action

This method allows us to execute an action if the `Optional` is present or another action if not.

### Don't

```java
Optional<Employee> employee = ... ;
if(employee.isPresent()) {
	log.debug("Found Employee: {}" , employee.get().getName());
} else {
	log.error("Employee not found");
}
```

### Do

```java
maybeEmployee.ifPresentOrElse(
    employee -> log.debug("Found Employee: {}",emp.getName()),
	() -> log.error("Employee not found")
);
```

### Note

- `ifPresentOrElse()` is similar to `ifPresent()` with the only one difference which helps to cover the `else` branch as well.

## Return Optional

In some cases, if the `Optional` yields present, then return an `Optional` describing the value; otherwise, return an `Optional` provided by the supplying function.

### Don't

```java
public Optional<String> getStatus(int id) {
    Optional<String> maybeStatus = ...
    if (maybeStatus.isPresent())
        return maybeStatus;
    else
        return Optional.of("Not started yet."); 
}
```

Don’t overuse the `orElse()` or `orElseGet()` methods to fulfill this type of operation because both methods return an *unwrapped* value.

```java
public Optional<String> getStatus(int id) {
    Optional<String> maybeStatus = ...
    return maybeStatus.orElseGet(() -> Optional.<String>of("Not started yet."));
}
```

### Do

```java
public Optional<String> getStatus(int id) {
    Optional<String> maybeStatus = ...
    return foundStatus.or(() -> Optional.of("Not started yet."));
}
```

### Note

- `or()` can throw `NullPointerException` if the supplying function is `null` or returns `null`. 

## Return Presence Status 

In some cases, you need to get an `Optional` status regardless of whether it is empty.

### Don't

```java
public boolean isEmployeeListEmpty(int id){
	Optional<EmployeeList> maybeEmployeeList = ... ;
	return !maybeEmployeeList.isPresent();
}
```

### Do

You can directly use `isEmpty()` method, which returns `true` if the `Optional` is empty since Java 11.

```java
public boolean isEmployeeListEmpty(int id){
	Optional<EmployeeList> maybeEmployeeList = ... ;
	return maybeEmployeeList.isEmpty();
}
```

### Note

- Nope

## Filter 

You can use filter on `Optional` objects.

### Don't

```java
if(employee != null && employee.getGender().equalsIgnoreCase("MALE")) {    
    ...
    // do something
}
```

### Do

```java
maybeEmployee
    .filter(user -> employee.getGender().equalsIgnoreCase("MALE")) 
    .ifPresent(() -> {    
    	...
        // do something
    });
```

### Note

## Extract & Transform

You can use `map` to extract and transform `Optional` object values.

### Don't

```java
if (employee != null) {
	Address address = employee.getAddress();
 	if (address != null && address.getCountry().equalsIgnoreCase("TN")) {
  		System.out.println("Employee belongs to TUNISIA");
 	}
}
```

### Do

```java
maybeEmployee
    .map(Employee::getAddress)
    .filter(address -> address.getCountry().equalsIgnoreCase("TN"))
    .ifPresent(() -> {
 		System.out.println("Employee belongs to TUNISIA");
	});
```

### Note

- The above code is well readable, concise, and efficient.

## Cascading Optional

`Employee#getAddress` is returning an `Optional`.

### Don't

```java
Optional<Optional<Address>> maybeAddress = 
            maybeEmployee.map(Employee::getAddress);
```

### Do

```java
Optional<Address> addressOptional = 
               maybeEmployee.flatMap(Employee::getAddress)
```

### Note

- The difference is that `map` transforms values only when they are unwrapped whereas `flatMap` takes a wrapped value and unwraps it before transforming it.

## Conclusion

We have seen what Java Optional is, its benefits, and dilemmas. Use it. But use it wisely. `Optional` is meant to be used as a return type. Trying to use it as a field type is not recommended. Additionally, we were able to better learn various antipatterns of `Optional` by studying some illustrative examples.