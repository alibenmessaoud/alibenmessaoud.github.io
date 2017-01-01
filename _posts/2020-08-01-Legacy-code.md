---
layout: post
title: Legacy Code, My Old Friend
tags: legacy best-practice
---

What is a legacy, a legacy system? A legacy system is an old method, technology, system, or application of, relating to, or being a previous or outdated computer system, yet still in the business; otherwise, it would not have survived.

When it was built at first, that legacy had an attractive design, and people used to say, let's add this and this and this as the business was expanding. Despite the undesired additions from the left and the right, the legacy remained and still working.

Nowadays, developers avoid working on these systems for many reasons, especially because the technical debt is much more than anybody could ever repay.

But what do we do when we have to fix a bug? Indeed, we copy and paste some code from other modules and get the system working again, but it is not a safe way to follow. Later other developers will do the same after checking the code. The last ones who did the commits have not respected the rules and implemented a fast bug fix, so the next developers probably will do the same. From there, it will get worse.

Another solution is to forget the old system and rewrite it from scratch. At first, you will say it is easy because someone did it before. Well, my boy, it is not easy. The code, design, or technology may look bad, but the knowledge in that code is an important part of all of this. Why? Because the code tells stories and rewriting a legacy from scratch will lead to losing much knowledge about the domain.

So what should we do? When I was a child, my parents used to fix broken things with glue. From that, I learned to fix and not throw things. Bringing a second life to something broken has made us proud each time.

Since many ages ago, humans have observed, learned from nature, and tried to solve their problems. A fig tree wraps around other old trees and replaces them. In this context, the strangler pattern is about the same thing. Similarly, we can replace the smelly lines of code with new clean, tested ones by applying the strangler pattern.

Indeed, the mix of old and new is much better than leaving the old one rot. Hence the system will continue to work, and if something incorrect happened or the latest version introduced bugs, we can roll back to the old code and behavior.

Let us be friends with legacy code.