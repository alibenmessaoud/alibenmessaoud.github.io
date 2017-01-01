---
layout: post
title: Understand the Three Laws of TDD
tags: best-practice rgr tdd test-driven-development
---

Uncle Bob described TDD with three rules in his article [The Three Laws of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd). These laws lock a developer into a cycle that is short but is very crucial to maintain. They are:

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

Simple but there's some repetition, you can go up and read them again!

Ok, stop, let me simplify them for you:

The first said **write production code only to make a failing unit test pass**.

The second said **write only enough of a unit test to fail**.

The third said **write only enough production code to make the failing unit test pass**.

It is clear that rule 3 implies rule 1 and have these two:

First, **write only enough of a unit test to fail**.

Second, **write only enough production code to make the failing unit test pass**.

The purpose of these laws is just to provide line-by-line granularity to the code. Almost every second you keep these laws into consideration.

Before finishing, take a Post-it and write down these rules and keep it next to your screens at the office. It will help to stay in the TDD cycle.