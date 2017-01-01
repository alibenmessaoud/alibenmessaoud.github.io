---
layout: post
title: Do You Want to Test Like a Pro?
tags: test tdd best-practice
---

Today's post is a set of notes from [Vicor Rentea ](https://youtu.be/1Z_h55jMe-M)about testing and doing it like a pro. This overview is a reference for myself, and I've just put it here. I will try in the next post to set up an article about Good programming tests' principles.

Let's start. 

To represent a test, we use the Given-When-Then style to specifying system behavior. So the words to retain are: Given, which means describe an set up the test fixture; When, means act as it calls the production code or the behavior to test; and Then, means assert and check the outcomes; the last step exists, but it is not usually listed, and it is called the annihilate or clean up;

When something is painful, but you can't avoid doing it .. what do you do? Do you postpone it? Delegate it? No, **you have to do it**, yes, you have to **bring the pain forward**!

> XP programming core concepts = {CI, pair programming, continuous refactoring, TDD} but they got lost in the agile movement!

Why do we write a test? to have coverage? No no !!
When you have problems with code, that slows the team, and .. you will blame management when the fault is actually yours! Because you didn't keep this beast under control. How do you keep the beast under control? You need a suite of tests you trust with your life!

- Why do we write a test? to get rid of the fear of willing to touch the code for years from now. Haha!
- So why do we write a test? to know when we're done.
- So why do we write a test? to read the specs before finishing the implm!
- So why do we write a test? Better design!
- So why do we write a test? Faster feedback!
- So why do we write a test? Save time!

In the medical domain, clinical tests must be sensitive, specific, fast, and few; for the same perspective, let's talk about test design goals; By Analog, tests must be:

- sensitive (fail for any bug, high coverage, correct); 
- specific (precise failure, expressive, isolated & robust, low overlap), 
- fast, 
- few (small, DRY); If you don't do that (specific, fast, and few), your test will not be maintainable! *Opposite of DRY is WET (Write Every Time) (We Enjoy Taping!)* 

Not maintainable? This means you will start to tinker, *bidouiller*! You will use @ignore, delete tests, delete asserts, comment tests, invert tests, hide things under the carpet, the misery! You will comment @Test (you'll commit and say it is a mistake, hahaha)!

Testing priorities: You should not test the getter and the setter, the easy part! You should test the code you fear, the hard and harder to understand!

- Weird logic: Do some isolation so you can avoid a scenario of a test that depends on other blocks of code! You have a bug, and before fixing it, you will have to write a test for it and for the next lines based on that code that can be a bug!

- Code based on for/ while/ if.
- Exception throwing.
- A method that calls two others.
- Trivial code!
- Legacy code with 0 bugs, 0 changes over the last year! Leave as it is!

Green test: The tests are green!
You made a huge PR or CR, and tests still green!
=> Evergreen test, you assert things that are not covered by tests! No trust, hahaha, don't trust!

How to check that your test covers what it says? 
=> You should make sure that the test is read after changing some production code! => Mutation testing (tests fail <=> prod code is altered): the idea is to make some change to the Production code, run test, check it is red, then you undo the changes. 

Tests, like laws, are meant to be broken! A test must be seen red a least once!

Why can a test fail? 

- regression [> GOLDEN RULE: the usual suspect = production code], 
- change request, 
- iso-functional changes [api changes, refactoring] so adjust the tests
- A bug in a test! Usually discovered via debugging -> fix the test 

> A bug in a test is like a knife in the back!

It would help if you had brainless explicit simple tests:
no concatenation, no arithmetic in assertions! `assertEquals("Greetings John", logic.getGreetings("John"));`
try-cath test: don't write a try-catch block in a test. The logic can fail and give NPE, and the code will not reach the catch block! Don't create new Exceptions classes and assert the returned codes and messages!

For loop for input-output params pairs, => use parameterized tests Ok! but you can use `.feature` files! A feature file can be signed directly by Biz guys.

Random test: red red green red ... replay the real calls from production to understand, [+golden master technique]

Multithread: looking for a race bug with sleep(random), hahaha! Look for a race condition in the test! Hahaha! Assert time! Yes, assert time! The test can control the time!

RW from files, DB, queues, WS, etc., do you test that? If UT fails because the DB is offline, then these tests are IT and not UT!

UT vs. IT: 
UT should be super-fast, only green, and break if a bug comes! 
IT: deep testing, slow, fragile!
Deep Testing: through many layers; Slow: start-up the container; Fragile (DB, files, external systems);  

Minimize duplication! Use a builder! Make builder, setter fluent on IJ! Use factory methods but don't share! Wrap the production API because it is ugly! Keep common stuff on fields, and use Shared @Before! hahaha! Everybody will add initialization there, and it will become trash! And when a test fails, you will have to understand the @Before! And you will have to try to find what part of @Before do you need? 

=> The solution is a fixture-based test breakdown! Different inner-classes or classes with @Before! Add the use-case in the name of the class like RecordRepo => RecordRepoTest => RecordRepoSearchTest + RecordRepoCriteriaTest! what happens if you break down the production code ?? hahaha, deal with it!

Test methods: 5 lines of code + expressive name! 
Use Given_When_Then in names!
`AnInactiveCustomerShould.beActivatedWhenHePlacesAnOrder()`

Expressive Tests
Assert collection content and not the size of the collection => opaque test! Make failure messages more beautiful!
Single assert rule means one feature/ test! So more than 1 assert by test! 

Respect encapsulation: yes! 
With fresh code: when you move things to refactor, private, public, extract, etc., tests should not break and keep green! 
With legacy code: *breaking encapsulation is a core technique*! You should have more tests to do some refactoring and not breaking existing new tests!
You run your test suite Test #13 is RED, so To debug it, you run it alone: Test #13 is GREEN ???
You run your test suite Test #13 is GREEN, so You add Test #43 but Test #13 turns RED ???

Hidden test dependencies: alone, a test is green; in the suite, it goes red! Why?? In-memory objects (static fields, singletons, power mocks, local threads, shared container); commit to DB (in-memo or external); external files! The solution is to reset them in @Before.

To Ensure Independence, JUnit runs tests in an unexpected order, so don't use `@FixMethodOrder(MethodSorters.NAME_ASCENDING)`

TDD: discipline in which you grow software in small increments to write the tests before the implementation. Run a red test for a new feature, Make it green using KISS YAGNI Golf no optimization just green.., then refactor using Clean code DRY without adding new feature ... then add a new feature by adding red test and so on... 

Rules of simple design by Kent Beck: 1/ passes the test 2/ reveals the intention 3/ no duplication 4/ fewest elements

Mocks fake objects: why we need it? To write fast tests (DB), to isolate at repetition, .. but there is confusion! 
Fake: stub responds to method calls (when/thenReturn) and mocks verify a method call (verify..)

Circle of purity: what can a UT check? 
Input + feature = Output; 									  ok but!
Input + feature /deps = Output; 						   ok but!
Input + feature /deps /side-effects = Output; 	ok!
But you must purify logic and have a simple design to avoid deps and side-effects.

example: 

```java
long placeOrder(uid, cart) {
	User user = userRepo.find(uid); // <-- stub userRepo
	Order order = new Order();
	order.setDelivery(user.getAddress());
	orderRep.save(order); // <-- mock orderRep
	return order.getId();
}
```

=> isolated where heavy logic is! 
=> keep the side effect out!

```java
long placeOrder(uid, cart) {
	User user = userRepo.find(uid);
	Order order = createOrder(user, cart);
	return order.getId();
}

Order createOrder(User user, Cart cart) {  // Mock free UT
  Order order = new Order();
	order.setDelivery(user.getAddress());
  return order;
} // = pure function
```

> => circles of infra and domain will be infra and domain and pure logic! 

with mocks and stubs, you can verify method call, how many times calls happen, or if no extra call happens, and capture-assert args! => this TDD Terror driven tests! Hahaha!

What to test!
query: ask for data, cmd: ask for mutation or side effect;
the object can: receive query or cmd as args, return query or cmd as a result, and process internal query or cmd; the question is what: I should test?
Assert query and cmd inbound, verify cmd outbound! The other ignore!
See the Minimalistic testing table!

|          | query  | Command |
| :------: | :----: | :-----: |
| inbound  | assert | assert  |
| to self  | ignore | ignore  |
| outbound | ignore | verify  |

Where to slice? 
A calls B calls C calls D! Where would you put the mock? 
Mock the nearest edge! or after ... but if B is not stable mock C! ... long setup for test ... 

A tale about the evil partial mock, aka spy!

Testing Legacy code: hard, need a broad vision, long...
Techniques for testing legacy code: 

- Extract & test (mock); 
- Break encapsulation (expose internal state, spy internal methods), 
- hack statics (control global state), 
- introduce seams (decouple places), 
- exploratory refactoring (break the design), 
- IDE refactoring (it cannot fail!), 
- UT + refactor; You don't UT, you refactor!

Key Points

- Test where the Fear is
- Correct Tests fail * Prod mutates
- Explicit, Simple Tests
- TODO: Try TDD!
- Purify the heavy logic
- Listen to Tests: Testable Design