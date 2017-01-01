---
layout: post
title: The Strangler Fig Pattern
tags: architecture best-practice
---

Since ages ago, humans used to observe and learn from nature to deal with life. A fig tree wraps around other old trees and replaces them. In this context, the strangler pattern is about the same thing and helps replace aging systems and bring new ones. 

## Context and Problem

Like the systems, technologies, tools, and architectures used to build these systems can become increasingly obsolete. Also, adding new features can increase these applications' complexity, making them harder to maintain or operate.

Replacing a complex system is not an easy task or project. During the migration process, we need to deal with two systems and migrate features gradually. Moreover, the systems' users have to know where a particular feature is located and need to be updated to point to the new location.

## Solution

Setup a new application and start replacing the features with new services in the new application. 

The idea is to create a façade to intercept requests going to the legacy system and route them either to the legacy or the new application.

Next, the features in the old system can be migrated gradually to the new system. Hence, clients will notice the change when they will be redirected to the new user interfaces. The new application must redirect them to the old system.

With the façade system, risks become accepted, and the development effort can be spread over time, particularly when we can not replace an old and complex system in one or two months. The routing helps to redirect the users safely to the "correct" services so teams can work on the migration with ease while ensuring the legacy application continuing to work.

Over time, when the new features are stable, the teams can disable the redirection, and the legacy system is eventually "strangled" and is no longer necessary.

Once the migration is complete, the legacy system can safely be retired.

## Issues and Considerations

There are many factors to consider and strategies to follow during the switching to the new systems.

Set up a plan to do migration and strangling by grouping common features and removing all unnecessary dependencies. This step is important for integrating the façade and the interception of requests and avoiding making it a single point of failure or a performance bottleneck. 

Does the new and old system share the same databases? I am not sure. The new applications always come with new features, so we will not have the same data schema. Data synchronization tools in the two ways are required to help switch back to the old schema of the old system easily when the recent versions of the new application are not stable or compatible.  

Security and data access are essential aspects, and the teams must provide a system that can work with the new and old systems.

## When to Use This Pattern

We can use this pattern when gradually migrating an application to a new architecture. This pattern may not be suitable when requests to the back-end system cannot be intercepted and smaller systems where the complexity of wholesale replacement is low.

