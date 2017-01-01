---
layout: post
title: Microservice Build Pipelines
tags: microservice build ci/cd pipeline
---

The main idea behind a microservices-based application is that systems are simpler to build and maintain when broken down into smaller pieces. Continuous integration (CI), delivery (CD), and deployment (also CD) have been advocated for years as a mindset for speeding up the integration of code changes with a “little & often” approach. This approach isn’t limited to microservices – it’s also used to deliver monolithic systems – but it is crucial in developing microservices-based applications.

Microservices are built and tested using CI/CD tools, where each microservice is treated as a small application. Once the code is committed in the repository, the CI/CD tools must trigger the build process:

1. Code compilation
2. Clean code verification 
3. Unit test execution
4. Contract/acceptance test execution
5. Building the application binary
6. Publishing the application binary to repository management
7. Building the application image if running environments is based on containers 
8. Publishing the application image to repository management, if
9. Deployment on running environments such as development, or quality assurance
10. Integration test execution
11. Functional test execution
12. Any other steps

The same steps are valid for continuous delivery (CD) and deployment (also CD). The particularity is the creation of the tag version in the repository.