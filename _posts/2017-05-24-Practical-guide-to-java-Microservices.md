---
layout: post
title: Practical Guide to Java Microservices
tags: java microservices jwt monolith spring patterns
---

Today’s applications are responsible for handling huge volumes of data for thousands of connected users at the same time. Thus the nature of the user experience demands fast applications and this is not possible with long delivery, large teams, and systems. Microservice architecture is an approach to distributed systems that promotes the use of finely-grained services instead of traditional tiered (multi-tier/ n-tier) architectures like monolith applications. In this guide, we will try to understand what microservices are, how to architect, and build them in Java.

To be able to understand the microservices, it makes a sense to start with the famous and classic architecture of any existing system in the world, the monolith, see what it is and its problems or advantages.

## The Monolith, the Cursed

For many years, traditional web or desktop apps are called "monolithic" because the entire thing is developed in one piece.

> To sum up, a monolith is the traditional application architectural style where all aspects and features of an application live in the same system.

Even if the application is well architectured or modularized, it’s always deployed as a single binary (JAR/ WAR/ EAR, etc. file). As a rough pointer, initially, a binary file will have a size in the range of 100Mb. At its core, there’s nothing evil with a monolith and it has historically been a favorite route because it’s simple to develop and run. But, it also comes with many challenges. We have seen over the years and will continue to see projects that the codebase have got or will get too big to **manage** or **understand** by a single developer. 

Because of these and many other accompanying problems, a new way of developing applications is coming to the surface. It is the microservices.

## What is a Microservice?

Microservices is an architectural style. It separates all of the main components of a monolith from each other as a series of tiny components called microservices, each running isolated its process on a separate or a shared machine and network. 

> Microservice is an application architecture defined by loosely-coupled services, where different features of the application live separately.

These components are built around business capabilities and independently deployable. 

Instead of theorizing about it, let's have an example to understand what is this thing. Imagine, we started building a Shopping app and we grabbed code from another project and we started developing a monolith app. 

Let's go and check one of its controllers. In Java code, `CustomerController` class looks like the following:

```java
// We've omitted some details 
// for the sake of clarity.

@Controller
class CustomerController {
	
  @Autowired
  AddressService addressService;
    
  @Autowired
  NotificationService notificationService;
    
  // ...
    
  @PostMapping("/customer/register")
  void register(RegisterForm form) {
    // other validation...
    validateAddress(form.getAddress());
    // etc..
    registerUser(form);
    notifyUser(form.getEmail());
  }
    
  void validateAddress(Address address) {
    // do stuff using addressService
  }
    
  void notifyUser(String email) {
    // do stuff using notificationService
  }
    
  // ...  
}

@Component
class AddressService {
    
  void validateAddress(Address address) {
    // do stuff with regex and local db
  }
}

@Component
class NotificationService {
    
  void notifyUser(String email) {
    // call Mail server
  }
}
```

In this controller, the logic behind the address validation is managed by `AddressService`. The same thing is valid for the notification where we can use the Java Mail API to send mails inside the `NotificationService`. Until now, the code runs in one JVM, one process on one server without errors. Nothing more, nothing less.

Let's say we want to do microservice. 

## How to Get This Java Monolith Controller Smaller?

The idea is to separate these behaviors and put them in different projects where each one of them is run on its own Java process.

> Big Bang or Strangler patterns are among the ways to do migration to microservices. We will talk about this subject in the next sections.

In practical terms, this means that instead of using `AddressService` and `NotificationService` in the methods `validateAddress()` and `notifyUser()` inside your `CustomerController`, we will move these classes with all its helper classes to its own Java project, put it under source control and deploy it independently from the original monolith.

Finally, we will end up with the Shop application, the original monolith without the `AddressService` and `NotificationService`, but with two extra microservices.

And now, the monolith is no longer responsible for address validation and sending notification. It has to call these microservices for address validation and sending notification. While it’s desirable to have small services, that should not be the main goal. Instead, we should try to decompose the system into services to solve domain problems. Some microservices might indeed be tiny, whereas others might be quite large.

So, we have tow small microservice and a monolith should call them. *How do we do that?*

## How to Communicate Between Microservices?

We have two styles of microservice communication: synchronous or asynchronous.

- Synchronous communication is usually done via HTTP and REST. HTTP is a synchronous protocol. The client sends a request and waits for a response from the service.
- Asynchronous communication is usually done through message brokers like RabbitMQ. The client usually doesn't wait for a response. It just sends the message to the broker.

A microservice-based application will often use a combination of these communication styles. 

In our Java app, we can use both varieties of communication in the address validation and notification microservices. Firstly, validating the address is a blocking operation and we need the result to continue the registration so we will use a synchronous microservice communication. Secondly, the notification is not a blocking operation and then we will use an asynchronous communication:

- For address validation, we will use the `HttpClient` or `RestTemplate ` class to call the address microservice. The address microservice will check the passed address in the parameters of the call and return code or response. 
- For notification, we will use the `AmqpTemplate` class to send a notification request to the notification microservice. The notification microservice will be triggered to send an email when a message is put in the queue.

Here's an example of code to do a synchronous call:

```java
...
// We've omitted some details 
// for the sake of clarity.
String validateAddress(Address address) {
  HttpEntity<Address> entity = 
      new HttpEntity<>(address, headers);
  
    return this.restTemplate
      .postForObject(config.getUrl(), 
                     entity, 
                     Address.class);
}
...
```

For messaging using Rabbit MQ, it is easy with Spring:

```java
...
// We've omitted some details 
// for the sake of clarity.
void notifyUser(String email) {
  this.rabbitTemplate
      .convertAndSend(
          "registration::success", 
           email
       );
}
...
```

The important detail to keep in mind is the fashion we will use to join the microservices. Ideally, we should try to minimize the communication between the internal microservices. Furthermore, having synchronous communication creates long request/response cycles.

## Deploying Microservices

Using microservices has many advantages, but that doesn’t mean it’s the natural option for everyone. Microservices need a much more complicated procedure to deploy. For example, automatization! It requires Ops members to ensure this automatization. Containers (Docker )are the suitable choice for a microservices architecture than other tools. They, by contrast, provide isolation at the level of the operating system. Besides, this architecture does not order the use of containers. Netflix, for example, runs its services on Amazon Web Services instances. But most organizations that adopt this architecture will find containers the adequate approach to run their applications.

So let's back to our shopping app and let's try to find out how to deploy our microservice-based app? We’ve got two options: either start from scratch with a Big Bang or use a method known as the Strangler Pattern. As we did before, we adopted the Strangler Pattern and I'll tell why in the next paragraph but let's read about these patterns first: 

- Starting from scratch means that we are recreating the existing application from scratch. All the teams will work on the new app and the legacy application will get ignored in the meantime.
- The Strangler Pattern is the favorite option for deploying microservices by, replacing a system feature step by step. Once a new piece is finished and deployed, it is put into use, and the old feature is “strangled”.

So, for our case, we started with two microservices, at least, and it will be easy to replace the address validation and the notification component. And the idea is to continue doing this with other features but not all the features. This opens up for another discussion about splitting a monolith and how to reduce communications and the number of dependencies.

The Strangler Pattern approach is advisable because it gives developers the freedom to use different technologies and languages, and because each microservice focuses on a single responsibility, we can develop more frequently and deploy different features together.

## Polyglotism

There is an approach to develop microservices: The idea is to let a team that is responsible for a particular set of microservices to determine which tech stack to use to solve domain problems. 

But developing Polyglot microservices could also grow complexity. It’s still common around Java guys that they have a problem with dynamic languages like Python and particularly hate JavaScript so developers from team A (doing Java all the time) will not come to rescue team B (doing NodeJS all the time) if there's a problem.

Also, diversity in teams can produce difficulties for testing, deployment, and develop difference in experience over all the teams.   

Hence, when you are not Google and alike, think twice about going this road. It might make your life worse. If you choose to take this path try smaller variety in the same programming language eco-system (Kotlin and Java and not PHP and Java).

## How to Test Microservices?

To test microservice, we will need to run all the microservice locally but it depends if we have enough memory on the machines. We’ll likely also need an up and running message broker and an email server to communicate with each other and this we'll add more resources to allocate and manage. This leads to a clear amount of underestimated complexity on the DevOps side. Testing is possible and we have the possibilities to plug the legacy components, production instances of the microservices but it is not a good idea to use data from the production environment. 

Fortunately, we have tools for stubbing and mocking calls. A mock replaces an object, and the microservice depends on with a test-specific object that verifies that the microservice is using it correctly. A stub replaces an object, and the microservice depends on with a test-specific object that provides test data to the microservice. The test data can be static or dynamic. Wiremock and Rest Assured are among the tools to do mocking or stubbing. 

Also, for persistence, we can use an in-memory database to replace a real instance of a database. Testcontainers are a great tool to run a database, messaging systems, and other tools in a containerized way.

## How to Make Microservices Writing Logs?

Unlike the monolith friend and instead of having one log file for the whole application, each microservice requires to write about its state, requests, failures, and responses to local storage. This will help to control the calling chain between microservices and not miss out on the opportunity to find out what caused an error! Following this idea, it's a good practice to tag each call with a unique ID (aka correlationID) that distinguishes the requests so it will be possible to follow the sequence of calls. Also, having a unique ID in the response payload of the request will help to identify problems more quickly at the customer side. Finally, it's necessary to send logs to a centralized location and system responsible for aggregating, filtering, and forwarding logs to some other processes or services.

## How Do Microservices Find Each Other?

As seen before, we need to reduce the communication between microservices and each one should be isolated and non-aware of the existence of the other microservices to ensure loose coupling. So each microservice needs to have its software-defined network that may or may not run on the same virtual computer. Also, IP addresses are dynamic and change each time we start a new instance of a microservice. So to keep the app working all the time, we need to pass by static names of microservices and machines instead of IP Address but we need to keep a mapping of the name of the microservice and its new IP address in a shared registry. The solution is the Service Discovery. Without the service discovery capability, the microservice App has to call validate-address microservice through an IP (e.g., 192.168.0.102/validate-address). This is a potential maintenance and deployment nightmare because you would have to track all the IP addresses inside the code, as well as refactor in the event of an IP change or deployment to another environment.

Through service discovery, microservices don’t need to be aware of the network. App microservice can invoke address validation through a simple URL HTTP endpoint (e.g., https://address-validation). With service discovery, any application can be ported to any environment without having to refactor the code. Libraries like Eureka or Zookeeper try to 'solve' these problems, like clients or routers knowing which services are available where. On the other hand, they introduce a whole lot of additional complexity.

## Security

Securing microservices can be accomplished in several ways. First, secure by design by securing code, validating the input, defining the responsibilities of each part of code in a microservice, and ensure confidentiality, data integrity, privacy, accountability, and availability. Second, use HTTPS everywhere, even for static sites. Third, within HTTPS,  JSON Web Tokens (JWT) have become very popular in the past several years and the first choice to secure web service endpoints. Fourth, use vaults to secure secrets. There are also other techniques to do like scanning Docker Configuration for Vulnerabilities, slowing down the attackers, scanning  the dependencies

## Key takeaways and Conclusion

Microservice is the evolution of the monolithic friend. It is like computers where people are and will continuously keep buying new computers with new hardware architecture and powerful CPU and GPU. As software engineers, we can not see the change inside the hardware but comparing i3 and i7 is not fair but the architecture is not the same thing. So this analogy is valid for Software architectures. Microservices work reasonably well for small applications: developing, testing, and deploying small monolithic applications are relatively simple.

The microservice architecture has several advantages. For example, individual services are easier to understand and can be developed and deployed independently. 

Also, has some significant drawbacks. In particular, applications are much more complex and have many more moving parts. 

Use Containers and Docker for everything!

That’s it for today. If you have any questions or if you found some errors (spelling, logical, whatever) just post them to the comment section or e-mail me. 