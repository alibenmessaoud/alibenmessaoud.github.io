---
layout: post
title: The Boy Scout Rule
tags: best-practice
---

The Boy Scout Rule can be summarized as: **Leave your code better than you found it.** Essentially, it means to leave our code a little better than how we found it when we started working on it.

Imagine you are working for a new e-commerce website and you worked on customer entity and you named it `Customer`. You did a great job as you chose a simple and good name, you implemented the feature well and it is 100% covered by tests.

The days pass, and the business started working with professional customers. A new programmer joined the team, and he is the responsible of implementing the logic behind the professional customers, so it’s time to revisit your code. Your implementations work great, but the website needs to manipulate professional customers like companies. Therefore adding another customer type just depreciated your code’s clarity. After the change, the website has `Customer` and `ProfessionalCustomer` entities. The names are not clear and we can not understand if `Customer` means the normal customer or anything else?

>  The act of leaving a mess in the code should be as socially unacceptable as *littering*. — [Robert C. “Uncle Bob” Martin](https://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule)

So the idea, with the boy scout rule in mind, is to rename for example the entity `Customer` to `PersonCustomer` or `CustomerAsPerson` or anything else giving some business signification to that entity. Hence, with that easy change, the system regains clarity.

All the code can be improved, and if we follow this rule we will end up with a codebase that continues to get better over the time.