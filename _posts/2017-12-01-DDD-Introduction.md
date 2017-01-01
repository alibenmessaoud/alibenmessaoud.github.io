---
layout: post
title: DDD - Introduction
tags: ddd architecture ddd-series
---

Good software design is as much the key to the success of a product as the offered functionalities; for example, Facebook provides a social networking platform and its architecture makes it different from other similar sites and contributes to its success. This shows how important a design is important to the success of a product.

Domain-Driven Design is a software design practice that centers on programming a domain to translate complex business problems into rich, simple, expressive and evolving software.

- Have you ever worked on a project with large teams where code are increasing each day?
- Have you ever asked yourself how to structure different types of classes and methods? Where to add these objects, DAO Objects, Mapper, Serializer, etc., in a messy project?
- What about the other the teams in the same company? How do they manage that complexity?

And many questions like these can be asked each day... 

But before going and see some details about DDD or 3D, let me tell you about some past experiences when I used DDD for the first time.

In 2015, I joined Sopra HR Software 😀 specialized in HR Systems, and it has two different, well-known solutions on the market HR Access and Pleaides. At that time, I started working on a new project, of course, a HR System, but to be plugged on the top of one of their legacies. The decision was to use DDD because the legacies were composed of several apps by a domain like a payroll, training, employees, etc., and it was suitable to follow DDD for the new system.

We worked on a pretty simple app 🤩. A manager or an employee could log in, check their profiles and the leaves, etc.

By the summer of 2016, the small app has become a real system, no fake or mock data were used. Moreover we passed from 2 teams to more than 8 teams of 4/5 developers 👷 working on different parts of this solution.

By the end if 2017, the solution covered all the domains with simple features, and it was composed of more than 400 jar files and a large codebase. It was insane. DRY was not respected as WET. We created a starter project generator with unused CRUD code 🤮. This experience was a great opportunity to see how a project based on DDD at the first year could fail and how to find a solution and save the ship.

With this entry, I would like to start a series of articles on the DDD. 

I do it for several reasons.