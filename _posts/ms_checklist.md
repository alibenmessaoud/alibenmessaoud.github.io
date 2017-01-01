Undoubtedly, microservices is the trending topic in the software development world.  Every organization is trying to decompose its application/product and convert into microservices, so that they can sell their product in name of microservices based architecture. However, are they really creating true microservices? or in the lack of understanding of overall microservice architecture Gamut, are they creating just another set of services, which can be called "Miniservices" to achieve the business needs. 

In this article, lets try to differentiate between a microservice, Miniservice, and Macro (Monolith) Service.

### Microservice 

You can call your service as microservice if and only if it is:

- Independently developed, deployed, and managed without having any awareness of the service around it.
- Communicates to each other through Publish-Subscribe Pattern.
- Has a Single Responsibility.
- Is loosely coupled.

![Microservices architecture](https://dzone.com/storage/temp/13820398-1596707256219.png)

So, if your service is not following any of these principles, then it is not a microservice, and you may be working on Mini-service, which I will explain shortly. Services are also not microservices if they:

- Share the Database (Physical or Logical).
- Communicate to each other in Synchronous fashion (Service to Service call using REST).
- Share the Infrastructure.
- Know more about what’s going on around it in order to communicate.

However, during development, not all developers understand the Pub-Sub model or lack on understanding of functionality, so they commit the following mistakes:

- Not aware with Pub-Sub or Message queue Integration pattern, hence they switch quickly into REST APIs, so that services can communicate.
- Not aware with complete business, hence mix-up the features and forget SRP of microservice.
- Do not physically segregate the Database for each service, instead create Schema in the same DB and many microservices that interact with this same Database

### **Miniservice** 

So what are Miniservices? It is like a group of microservices that come together in a pattern to resolve a business need. It is a single function as a service.

You can call your service as Miniservice if:

- If multiple applications share the same database.
- Services communicate with each other through REST APIs and do not embrace event-based architecture for asynchronous communication.
- Share infrastructure for deployment.

![Miniservices architecture](https://dzone.com/storage/temp/13820430-1596708655731.png)

Now, if all your service domain services share a single database and application server and call each service through direct calls, as they all are deployed in the same JVM, then your application is a Monolith (Macroservice).

### **Marcoservice (Monolith)** 

It is nothing but a Monolith application where all business services are deployed as a single package in the application server and share the same database (Physically and logically). It has less complexity and embraces tight coupling between services. 

![Monolith architecture](https://dzone.com/storage/temp/13820433-1596708913289.png)

## **When to Use What** 

It all depends on your business need or project requirements. When you have your functionality in multiple code repositories, then creating microservices makes sense. On the other hand, if you have your multiple functionalities in the single code repository or one service has multiple functionalities, then Miniservices is the solution.

When complexity is increasing, and you don't need to communicate with other services, then you can think of writing Miniservices. If you have independent functionalities in the project which need asynchronous communication between them, then write microservices. 

Mini-Services are cost-efficient, however microservice or less cost-efficient as we have to have multiple functions deployed in real time to achieve the business goal.

## **Conclusion** 

Teams and developers are decentralizing their application as much as they can, however not writing as many microservices as you might think. Many are still writing combinations of microservices and Miniservices. 