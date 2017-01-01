---
layout: post
title: Code Review
tags: best-practice
---

In our daily tasks, designing, coding, and deploying are not the only things to do every day. There are also planning meetings and technical discussions to do with the other members and teams. And also code reviews (CRs). In this article, we will see what, why, how, and when to do code review.

## Motivation

It is not about simply fixing errors in code but also to share knowledge and build common coding guidelines. Rather than looking for errors, you should review the code by trying to learn and understand it.

## What Is a Code Review?

Code review is a methodical study of source code. It is dedicated to locate and repair errors missed in the initial development stage and to share knowledge and build common coding guidelines.

## Roles and Responsibilities

1. Developer/ Reviewee: the authors and the initiator of the review request/ merge request/ pull request.
2. Reviewer: the people who will review the code and report the findings to the developer.

## Why Code Reviews Are Important?

A delivered feature is the outcome of the collaboration of developers, architects, testers, ops guys, domain owners, etc. But the heart of the matter is the code because it translates the result of that collaboration for a customer request. So this phase can not be neglected like this. Indeed, it helps to keep a high quality of code with fewer bugs. Keeping that highness of quality helps to speed up the process from design to delivery. How? I'll tell you. 

> [...] software testing alone has limited effectiveness. — Steve McConnell, Code Complete

In most organizations code reviews follow a rigid and formal process. Often, the reviewers are the architect or a lead developer. Reviewers require both the time to read the code and the time to keep up to date with all the details of the system and changes; they can quickly become the bottleneck in this process, and the process soon deteriorates which will cause sad customers at the end.

## The Main Goals Of Code Reviews

The main purposes of code review are:

- To find and fix bugs early in the process
- Better-shared understanding of the codebase
- Maintain a level of cohesion in design and implementation
- Identify common errors across the team thus reducing rework
- Gives a different view. “Another set of eyes”, and provide the distance needed to recognize problems
- Team cohesiveness

## When to Code Review?

Have a regular code review day each week after automated checks (tests, style, other CI) have completed successfully. Spend a couple of hours in a review meeting or alone. 

## Review What?  

The main areas a reviewer is focusing on are:

- CI
- Styles
- Formatting Conventions
- Comments
- Unit Testing and Coverage
- Coding Conventions
- Data structures
- Logic and Functional 
- Error Handling
- Hardcoded and development only things
- Resource Leaks
- Thread Safety and deadlocks
- Control Structures
- Performance
- Reusability 
- Security

## Tips for the Reviewee

### When Submitting Code Reviews

The author is responsible for submitting CRs that are easy to review and not to waste reviewers’ time and motivation:

- Only submit complete, self-reviewed, and self-tested CRs after automated checks (tests, style, other CI), documented
- Reviewers are not reviewing to solve your problems and do your work 
- Changes should have a narrow, well-defined, self-contained scope that they cover exhaustively
- Have checked and covered the areas where the reviewer will focus on
- Understand and accept the fact of making mistakes
- Link to the code review from the ticket/story
- Description and how to test
- Keep one commit

### After/ During Code Review

Remember that the whole point of a review is to find problems, and problems will be found:

- No matter how much you know, someone else will always know more
- Fight for beliefs and accept gracefully the defeat
- Justify why the code exists
- Help to maintain the coding standards. Respect them
- Not take it personally when one problem is found 
- Don’t be offended by your reviewer’s recommendations
- Take recommendations seriously even if you don’t agree
- Accept that many programming decisions are opinions
- Ask good questions: to avoid judgment and avoid assumptions
- Don't make requests
- Ask for clarification
- Be explicit
- Be humble
- Don't exaggerate
- Don't use irony
- Be grateful for the reviewer's suggestions
- Push commits based on earlier rounds of feedback as isolated commits to the branch
- Try to respond to every comment

## Tips for the Reviewer

Understand why the change is needed (fixes a bug, improves the user experience, refactors the existing code). Then:

- Be kind to the coder, not to the code
- Make sure you have good coding standards to reference
- Remember that there is often more than one way to address a solution
- Treat people who know less than you with respect, deference, and patience
- Ask questions rather than make declarations
- Avoid the “Why” questions
- Provide feedback only
- Remember to praise
- Comments: concise, friendly, actionable
- Communicate which ideas you feel strongly about and those you don't
- Identify ways to simplify the code while still solving the problem
- Offer alternative solutions, but assume the author already considered them
- Seek to understand the author's perspective
- Sign off on the pull request with a 👍 or "Ready to merge" comment

## Conclusion

We looked closer to the code review process. Code reviews play a key role when it comes to boosting efficiency and quality. Specifically, taking advantage of the right code review actions is what helps you to improve team members’ expertise first.