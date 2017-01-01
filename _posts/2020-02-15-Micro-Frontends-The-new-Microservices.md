---
layout: post
title: Micro Frontends, The New Microservices
tags: java microservice architecture frontend
---

In the last years, microservices have got much attention. Several companies adopted them, allowing teams to scale various applications independently and quickly and bypass large monolithic backends' constraints. Sadly, we've also seen many teams continuing to cope with a monolithic frontend — a big browser beast cursed application that sits on top of the backend components.

### Some History

Micro Frontends first came up in [*ThoughtWorks Technology Radar*](https://www.thoughtworks.com/radar/techniques/micro-frontends) at the end of 2016 and, lately, in November 2019. [micro-frontends.org](http://micro-frontends.org/) defines a Micro Frontends as: "A composition of features that are owned by independent teams. Each team has a distinguished area of business it cares about and specializes in. A team is cross-functional and develops its features end-to-end, from the database to the user interface."

In other words, Micro Frontends extends the principles of microservices to the frontend. Thus, a website or a web application can be considered a composition of features owned by independent teams from the database to the user interface.

### Problems With The Monolithic Frontend

Today, frontend applications are in the same position as their backend counterpart when microservices came into the picture late in 2012. The difficulties look the same but at the frontend level where large teams work on the same codebase and require dealing with problems like overriding of styles without knowing, breaking APIs changes, and never forget to consider the updates made by the backend teams that causes more and more pain.

### Solutions With Micro Frontends

*Jonas Boner* describes the [five primary characteristics of microservices](http://jonasboner.com/bla-bla-microservices-bla-bla/): isolation, autonomy, single responsibility, an exclusive state, and mobility. By analogy, switching to a micro frontends approach can guarantee the latter characteristics and solve some of those problems. It becomes simple to deploy in small parts with Micro Frontends and speed time-to-market the frontend same team's frontend and backend components. As well as their ability to improve the applications' design, applying this approach means a huge change of teams' mindset for the development.

### An Example Of Micro Frontends

Zalando is not only selling clothes, but it develops libraries too. Zalando Tech department uses microservices and Micro Frontends too to provide a better user experience. Let's take a look at [zalando.fr/](https://www.zalando.fr/)

Let's pretend that there are 4 teams (team 1, team 2, team 3, and team 4).

- Team 1 works on the profile page, session, and basket using Angular.
- Team 2 works on the navigation and the search using React.
- Team 3 works on the list of products and its detail using Vanilla JavaScript and some jQuery.
- Team 4 works on assembling all the last mentioned features on zalando.fr/.

As you can observe, each team is responsible for a small feature.

### The Benefits Of Using Micro Frontends

First, using this style, teams will have smaller and maintainable codebases instead of a monolith. Second, each team can work autonomously and adopt diverse technologies as they need for their domain's frontend and backend. Third, an owner team of a domain can deploy without worries.

### When To Do Micro Frontends

When you create a big application, you work with large teams and use microservices as your backend architecture, and you could surely benefit from the Micro Frontends approach.

### Who Uses Micro Frontends

Various companies adopt Micro Frontends like Airbnb, Skyscanner, Zalando, OpenTable, SAP, New Relic, Saagie, Spotify, IKEA, Dazn, etc. Moreover, it can be realized in different ways and using many patterns and libraries such as [LitElement](https://lit-element.polymer-project.org/), [Svelte](https://svelte.dev/), [Stencil](https://stenciljs.com/), https://angular.io/guide/elements, [Polymers](https://www.polymer-project.org/), [Mosaic](https://www.mosaic9.org/), [OpenComponents](https://opencomponents.github.io/), [Open Web Components](https://open-wc.org/). You can watch the video of [Erik Dörnenburg on Youtube](https://www.youtube.com/watch?v=A3n1n5QRmF0) to know more about patterns.

### Challenges

The most important challenge that arises with using this style is consistency. With many teams responsible for the different pieces that make up a site or an application, style and branding can diverge over time. The second challenge is performance if too many components are imported and dependencies are not optimized.

### Conclusion

Micro Frontends may not be the most suitable solution for each problem, but they can propose much value for enterprise projects. It could be one of the best ways to handle large codebases and teams as it helps keep the delivery fast, up-to-date, and flexible. I recommend you visit [micro-frontends.org/](https://www.alibenmessaoud.com/Micro-Frontends-The-new-Microservices/micro-frontends.org/).