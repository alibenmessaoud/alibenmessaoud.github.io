---
layout: post
title: SOLID / IDEALS - Microservice Design Principles 
tags: microservices design best-practice solid
---

As developers, we build software and make decisions daily that can influence the system and the developer experience. Wrong changes in the code can result in bugs and bad user experience. For these reasons, we should follow SOLID Principles. And at the project level, the same thing is valid for the architecture of the system, especially with an ever-increasing number of tools and frameworks around microservices.

SOLID can help a developer to write better code. So what about the application designing and coherence at runtime? Some of the SOLID principles apply to microservices but don't cover the whole spectrum of design decisions. Thus, the SOLID principles for microservices are IDEALS.

"DEALS" are core principles for microservice design: 

- **I**nterface segregation: tells us that different types of clients (mobile, web, CLI) should be able to interact with services through the contract that best suits their needs. 
- **D**eployability (is on you): acknowledges that there are critical design decisions and technology choices developers need to make regarding packaging, deploying, and running microservices.
- **E**vent-driven: suggests that services should use asynchronous messages or events instead of the asynchronous call.
- **A**vailability over consistency: reminds us that more often, end users value the availability of the system over solid data consistency, and they're okay with eventual consistency
- **L**oose coupling: remains a critical design concern in the case of microservices, with respect to afferent (incoming) and efferent (outgoing) coupling.
- **S**ingle responsibility: enables modeling microservices that are not too large or too slim because they contain the right amount of cohesive functionality.

Following the IDEALS is not a magic elixir that will make a microservice design successful. As always, we need to have a good understanding of design patterns and architecture tactics.

Reference: https://www.infoq.com/articles/microservices-design-ideals/