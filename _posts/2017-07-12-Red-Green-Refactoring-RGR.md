---
layout: post
title: Red Green Refactoring
tags: best-practice rgr tdd test-driven-development
---

To execute Test-Driven Development (TDD) properly we need to learn its key methodology Red, Green, and Refactor (RGR). This method consists of 3 phases that are repeated continuously: *Red*, *Green,* and *Refactor*.

TDD is a simple paramount ‘‘philosophy’’ to coding. So the idea behind TDD is simple - you write a code aiming to fail, and then you adjust it to make the test pass. Clean and repeat to achieve satisfaction. Easy.

RGR applies that theory into three concrete steps:

1. The Red step - write a unit test that you are certain will fail, so this means that you have to have a failing test first. You can’t write any production code before ‘‘red’’.
2. The Green step - check your failed code, inspect the failure/error logs linked to that code, and then do the modifications so that your unit test will pass, or go green! 
3. The Refactor step - when it is green, the next move would be to refactor the code to be clear, beautiful, and most importantly faster to run. Principles such as Do not Repeat Yourself (DRY) are welcome in this step.
4. Repeat until satisfied. That’s it!

There are two rules to respect

- [ ] The cycle must be very short - about 5 minutes!
- [ ] *Only* refactor in the GREEN!

But ... Why?

RGR, as a way to approach the code, helps to uniformly analyze and rethink hypotheses, so the code will contain fewer bugs as we can react to problems as they occur.

Focusing on writing individual good test specs for each feature or scenario and making them pass, then the code will become more developer-friendly, functional, and smooth.

Finally, it is the simplest way to achieve both good quality code and test coverage.