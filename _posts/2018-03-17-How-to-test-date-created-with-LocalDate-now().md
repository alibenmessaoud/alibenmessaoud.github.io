---
layout: post
title: How to test date created with LocalDate.now()?
tags: java test java8 date mock
---

Once, I needed to test time-sensitive methods because there was a logic around the current date in my code. In these methods, I am using `LocalDate ` Java classes. 

## Real situation

My class, as an example, was `Drug`, a simple *POJO*, and contains `isValid` method to check the `expirationDate` attribute against the current time.

```java
public class Drug {
    private final String name;
    private final LocalDate expirationDate;

    public Drug(String name, LocalDate expirationDate) {
        this.name = name;
        this.expirationDate = expirationDate;
    }

    public static Drug of(String name, 
                          LocalDate expirationDate) {
        return new Drug(name, expirationDate);
    }

    public boolean isValid() {
        return expirationDate.isAfter(
            LocalDate.now().minusDays(1)
        );
    }
}
```

To write a unit test, we will code the same minimal lines to assert that the expiration date is valid of a new object `Drug`. Imagine that we had written that test on *December 2, 2018* and we had set the expiration date to *December 31, 2018*. Then, 2018 ended and you had come back on *January 2, 2019* to work on another feature.

```java
@Test 
void should_validate_expiration_date() {
	// LocalDate.now() is 2018-12-01
    Drug drug = Drug.of("Drug1", LocalDate.of(2018, 12, 31));
    
    // fails after January 1st, 2019
    assertThat(drug.isValid()).isTrue(); 
} 
```

The new feature build will fail not because of the new code but because the expiration date of that test is not valid. Therefore to get rid of this problem, some people set the expiration date to 2019-6-30. Surely, it will fail again in six months. After reading code, the first idea that comes to the mind is to mock the date. So let's do it;

```java
@Test 
void should_validate_expiration_date_with_mock() {
    // LocalDate.now() is 2019-01-02
    when(LocalDate.now()).thenReturn(LocalDate.of(2019, 1, 1));
    Drug drug = Drug.of("Drug2", LocalDate.of(2018, 1, 30));
    assertThat(drug.isValid()).isTrue();
} // the mock call will fail  because now() is a static method.  
```

Mocking *now()* method will fail because it is a static method and will show this message:

```java
org.mockito.exceptions.misusing.MissingMethodInvocationException: 
when() requires an argument which has to be 'a method call on a mock'.
For example:
    when(mock.getArticles()).thenReturn(articles);

Also, this error might show up because:
1. you stub either of: final/private/equals()/hashCode() methods.
   Those methods *cannot* be stubbed/verified.
   Mocking methods declared on non-public parent classes is not supported.
2. inside when() you don't call method on mock but on some other object.
```

## Solution 1

If you don't care about adding coupling to your class, a better approach could be to provide a `Supplier` to your class as next:

```java
public class DrugWithDateSupplier {

    private final String name;
    private final LocalDate expirationDate;

    private final Supplier<LocalDate> dateSupplier;

    public DrugWithDateSupplier(String name, 
                                LocalDate expirationDate,
                                Supplier<LocalDate> dateSupplier) 
    {
        this.name = name;
        this.expirationDate = expirationDate;
        this.dateSupplier = dateSupplier;
    }

    public static DrugWithDateSupplier of(String name, 
                                          LocalDate expiationDate) 
    {
        return new DrugWithDateSupplier(name, expiationDate, LocalDate::now);
    }

    public static DrugWithDateSupplier of(String name, 
                                          LocalDate expiationDate,
                                          Supplier<LocalDate> dateSupplier) 
    {
        return new DrugWithDateSupplier(name, expiationDate, dateSupplier);
    }

    public boolean isValid() {
        return expirationDate.isAfter(dateSupplier.get().minusDays(1));
    }
}
```

This way, it will be easy to create a `Supplier` for testing purposes in the test. Like this, we set the current date to a specific value if we pass a supplier.

```java
@Test 
void should_validate_expiration_date() {

    DrugWithDateSupplier drug = DrugWithDateSupplier.of(
        "Drug1", 
        LocalDate.of(2018, 12, 31),
        () -> LocalDate.of(2018, 12, 30)
    );
    assertThat(drug.isValid()).isTrue();
}
```

Now your test will go green forever.

## Solution 2

Instead of coupling a supplier, consider "a date/time provider" as a dependency, and inject at usage. The `java.time` package includes an abstract class `java.time.Clock` to allow alternate clocks to be plugged in as and when required. With that, we can plug our implementation or find one that is already made to satisfy our needs. In the test, we can provide a fixed clock via `Clock.fixed(...)` and for production, we can use `Clock.system(...)`. In other words, it's possible to simulate running in the future, in the past, or in any arbitrary point in time:

```java
// go to the future:
Clock clock = Clock.offset(constantClock, Duration.ofSeconds(10));
        
// rewind back with a negative value:
clock = Clock.offset(constantClock, Duration.ofSeconds(-5));
 
// the 0 duration returns to the same clock:
clock = Clock.offset(constClock, Duration.ZERO);
```

So the class `Drug` will be as follows: 

```java
public class DrugWithClock {

    private final String name;
    private final LocalDate expirationDate;

    private final Clock clock;

    public DrugWithClock(String name, 
                         LocalDate expirationDate, 
                         Clock clock) 
    {
        this.name = name;
        this.expirationDate = expirationDate;
        this.clock = clock;
    }

    public static DrugWithClock of(String name, 
                                   LocalDate expiationDate) 
    {
        return new DrugWithClock(name, 
                                 expiationDate, 
                                 Clock.system(ZoneId.systemDefault())
                                );
    }

    public static DrugWithClock of(String name, 
                                   LocalDate expiationDate, 
                                   Clock clock) 
    {
        return new DrugWithClock(name, expiationDate, clock);
    }

    public boolean isValid() {
        return expirationDate.isAfter(
            LocalDate.now(clock).minusDays(1)
        );
    }
}
```

Like this you set the inject a specific date as seconds in the test: 

```java
 @Test void should_validate_expiration_date() {

    DrugWithClock drug = DrugWithClock.of(
        "Drug1", 
        LocalDate.of(2018, 12, 31),
        Clock.fixed(
            Instant.ofEpochSecond(1546128000), 
            ZoneId.systemDefault()
        )
    );
    assertThat(drug.isValid()).isTrue();
}
```

Now the test will go green forever. But this solution may fail because it plays with the `Clock` class. Among the side effects of this solution is when the test occurs at the point of a daylight saving time change or if the system clock gets changed by another behavior.

## Conclusion

In this article, we’ve explored different ways to test the time/date created with `LocalDate` Java classes.