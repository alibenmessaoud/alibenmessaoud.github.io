---
layout: post
title: Continuous Delivery - Small Batches and Risk
tags: best-practice ci/cd
---

In this article, we'll talk about the principles of continuous delivery, and we'll focus on the Work in Small Batches principle and how it helps reduce risks.

There are five principles at the heart of continuous delivery:

- Build quality in
- Work in small batches
- Computers perform repetitive tasks, people solve problems
- Relentlessly pursue continuous improvement
- Everyone is responsible

In traditional software development approaches, handoffs from dev to IT ops consist of whole releases. Isn't that risky? Imagine after a month of development and testing, bugs still occur in the production environment. Even taking more time with testing to guarantee confidence in the package is not enough. The risk will be there all the time, and the idea is to find a way to reduce it as we can not eliminate it definitely. Let's think about what we mean by risk.

In simple terms, the risk is the possibility of something bad happening. The classic formula of risk is the probability of occurrence of an unwanted event multiplied by the consequence (loss) of the event:

- `Risk = probability of an unwanted event × consequences` 


Let's translate it to something related to software development and running on production:

- `Risk = probability of failure × consequences` 

Risk in software arises from the potential for an unwanted failure resulting from an event determined by its probability and the associated consequences. 

Therefore, the risk is low when it includes low values of its factors, probability of failure, or consequences, and it is high when both sides of the product are high.

So packaging a long list of changes and set them for release and production increases the probability of failure and high values of consequences. I'll not speak about production problems like a server outage, defect cascading, loss of data, etc. To reduce this risk, we need to shorten that list of features to go live after a sprint. We can do this with small release cycles or small batches. Surely, releasing more often, once or twice per week, is introducing more volatility into production. Still, it has many benefits, and it is one of the key goals of continuous delivery. Therefore, it reduces the time it takes to get feedback on our work, makes it easier to triage and remediate problems, increases efficiency and motivation, and prevents us from succumbing to the sunk cost fallacy.

There's no way to reduce the impact of a failure—the worst case is that the release could bring the whole system down and incur severe data loss—but we lower the overall risk with the smaller releases.

Small Batches principle often reduces the probability of a failure and, therefore, the risk of change.