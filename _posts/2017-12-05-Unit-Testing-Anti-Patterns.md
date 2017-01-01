---
layout: post
title: Unit Testing Anti-Patterns
tags: test best-practice patterns
---

Software development consists of many stages and if bugs are caught in the earlier stages it costs much less to fix them. That is why it’s important to get testing done as soon as possible. In this article, I'll write about unit testing anti-patterns because they also exist, and there are many. All these patterns are based on several posts like [stackoverflow.com](https://stackoverflow.com/questions/333682/unit-testing-anti-patterns-catalogue).

## 1. Terminology

There many versions of the testing pyramid but I will focus on the definition of the test pyramid as presented below. 

```
               '
              /=\
             /===\
            /=====\
           /==E2E==\
          /=========\
         /===========\
        /=INETGRATION=\
       /===============\
      /=================\
     /=======UNIT========\
    /=====================\
```

Therefore the two major test categories mentioned as unit and integration tests from now.

| **Test**             | **Focus on**       | **Require**                | **Speed** | **Complexity** | **Setup needed** |
| -------------------- | ------------------ | -------------------------- | --------- | -------------- | ---------------- |
| **Unit test**        | class, method      | the source code            | very fast | low            | No               |
| **Integration test** | component, service | part of the running system | slow      | medium         | Yes              |

- **Unit tests** are the tests that accompany the source code and have direct access to it. In these tests, we test a single class/method/function and anything else is mocked/stubbed.
- **Integration tests** also called service tests, or even component tests focus on a whole component. Compared to unit tests they may require more specialized tools either for preparing the test environment or for interacting/verifying it.

## 2. Software Testing Anti-Pattern List

- **Cuckoo, aka Stranger**: a test method that sits in the same unit test class or file but doesn't belong there.
- **Test-per-Method**: tests exist in a one-to-one relationship between test and production classes.
- **Anal Probe**: a test method that use unhealthy ways to perform its task, such as reading private fields using reflection.
- **Conjoined Twins**: tests that people are calling "Unit Tests" but are integration tests since they are not isolated from dependencies (file configuration, databases, etc.)
- **Happy Path, aka Success Against All Odds, Liar**: The tests stay on happy paths without testing boundaries and exceptions.
- **Slow Poke**: a unit test that runs incredibly slow.
- **Giant**: a unit test that contains many many test cases. 
- **Mockery**: a unit test that contains so many mocks, stubs, and/or fakes that the system under test isn't even being tested at all, instead data returned from mocks is what is being tested.
- **Inspector**: a unit test that knows so much about what is going on in the object to achieve 100% code coverage so any attempt to refactor will break the existing test.
- **Generous Leftovers, aka Chain Gang, Wet Floor**: an instance where one unit test creates data that is persisted somewhere and another test reuses the data for its devious purposes. 
- **Local Hero, aka Hidden Dependency, Operating System Evangelist, Wait and See, Environmental Vandal**: a test that is dependent on something specific to the development environment it was written on.
- **Nitpicker**: a test that compares unimportant details output when it's only interested in small parts of it.
- **Secret Catcher**: a test that at first glance appears to be doing no testing due to the absence of assertions, but it is relying on an exception to be thrown.
- **Dodger**: a test that has lots of minor and easy tests but never tests the core desired behavior.
- **Loudmouth**: a test that clutters up the console with diagnostic messages, logging, and other miscellaneous chatter, even when tests are passing.
- **Greedy Catcher**: a test which catches exceptions and swallows the stack trace, sometimes replacing it with a less informative failure message, but sometimes even just logging and letting the test pass.
- **Sequencer**: a test that depends on items in an unordered list appearing in the same order during assertions.
- **Enumerator, aka Test With No Name**: a unit test that its name is only an enumeration, e.g. test1, test2, test3.
- **Free Ride, aka Piggyback**: add a new assertion along in an existing test case instead of writing a new test case method. 
- **Excessive Setup, aka Mother Hen**: a test that requires a lot of work to set up to even begin testing. 
- **Line hitter**: tests that merely hit the code, without doing any output analysis and code coverage tools confirm it with 100%.  Paying excessive attention to test coverage.
- **Forty-Foot Pole Test**: tests that act at a distance, separated by countless layers of abstraction and thousands of lines of code from the logic they are checking.
- **Scornful**: treating test code as a second class citizen.
- **Unit and integration together**: Unit tests without integration tests or the reverse.
- **Wrongy**: a test that is wrong or test the wrong functionality.
- **Laboring**: tests that run only manually
- **Impractical**: Not converting production bugs to tests.
- **The Test It All**: tests should not break the Single Responsibility Principle.
- **Wait and See**: A test that runs some set up code and then needs to 'wait' a specific amount of time before it can 'see' if the code under test functioned as expected. 
- **The Dead Tree**: A test which where a stub was created, but the test wasn't written.
- **The Flickering Test**: A test which just occasionally fails, not at specific times, and is generally due to race conditions within the test.
- **The Sleeper, aka Mount Vesuvius**: a test that is destined to fail at some specific time and date in the future.
- **The Butterfly**: a test that contains data that changes all the time, like a structure which contains the current date, and there is no way to nail the result down to a fixed value.
- **Doppelgänger**: a test that copy some of production code in the test or another class to do the test. 

## 3. Conclusion

In this article, we seen Unit Testing Anti-Patterns. It is possible to find redundant patterns or repeated ones with other names. If you spot an error, please comment.
