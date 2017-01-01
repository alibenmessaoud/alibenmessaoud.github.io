---
layout: post
title: An Introduction to Vert.x
tags: vert.x 
---

According to vertx.io/, Vert.x is a toolkit for building reactive applications on the JVM. Its essence is processing asynchronous events, mostly from non-blocking I/O, and a threading model based on events processed in an event loop. Moreover, we call it polyglot due to its support for multiple JVM and non-JVM languages like Groovy, Ruby, Python, and JavaScript.

However, to better understand this model's use, let's take a step back and see the asynchronous programming concept.
In the early days of computing, all programming was single-threaded. Computers ran a single job at a time. When a program was running, it had exclusive use of the computer's time. A program usually sequentially executes instructions and waits for every instruction to finish before executing the next ones. We call it synchronous execution.

With the invention of modern multi-CPUs and multitasking in operating systems, users started to do multiple tasks. Hence, multithreading comes into the picture as an extension of these advances. Computers became able to do concurrent/parallel execution of more than one sequential set (thread) of instructions. For example, we can write on a text editor and have a text file loading on another window. The style remained synchronous because there was always a main thread for each application and this thread was designed to wait for the result from the other threads before executing the next operations.

Asynchronous programming comes into play to solve this problem by allowing to perform actions as soon as a specific instruction becomes available. 

Asynchronous programming is about non-blocking execution between functions, and we can apply asynchronous with single-threaded or multithreaded programming.
Let's take a simple analogy; Suppose two friends X and Y are preparing pasta with sauce. They don't have pasta. X goes to the market to buy pasta. Does Y freeze right there, waiting for the pasta? No. Y prepares the sauce. Also, while waiting X to come back, Y boils the water. X comes back home, and the sauce is almost ready, and the water is hot. X puts the pasta. When it is done, X serves the dinner. Then, X and Y eat.

Multithreading is about workers, and Asynchronous is about tasks.

Let's go back to the main topic of this article Vert.x. Vert.x implements the Reactor design pattern and many other techniques to provide an asynchronous programming model to build fast, reactive, and scalable applications.

The main concepts behind Vert.x to achieve high performances are Event Loop, Event Bus. Things based on Multithreading and Asynchronous. 

The event loop is the implementation of the Reactor design pattern.
The main idea behind the reactor design pattern is to have a set of handlers (which in Node.js is represented by a callback function) associated with each I/O operation, which will be invoked event is produced and processed by the event loop. Vert.x uses this pattern and implements a multi-reactor pattern where, by default, each CPU core has two event loops for higher performances. Another concept linked to the event loop is the handlers. The handlers are the code that does the job called verticles.

Verticles are the main actors when developing a Vert.x application and are deployed randomly on an event loop at the startup time. It helps to process events using an Event Loop on a specific Thread.
The second concept is the event bus.

The event bus is the communication channel that Verticles used to send messages asynchronously and in a publish-subscribe manner. Verticles are registered to the event bus and given an address to listen to. The event bus allows verticles to be scaled, as we only need to specify what address a verticle listens for events on and where it should publish those events.

Now we'll start an application:

```java
public class HelloWorldVerticle extends AbstractVerticle {

    @Override
    public void start() throws Exception {
        vertx.createHttpServer() 
            .requestHandler(req -> req.response() 
                .putHeader("content-type", "text/html") 
                .end("<html><body><h1>Hello World</h1></body></html>"))
            .listen(8080);
    }

}
```

It is good to listen on port 8080, but we need something real. Let's add a route. The start method becomes:

```java
@Override
public void start() throws Exception {

    final Router router = Router.router(vertx);

    router.get("/hello/") 
        .handler(req -> req.response()
            .putHeader("content-type", "text/html") 
            .end("<html><body><h1>Hello World</h1></body></html>"));

    vertx.createHttpServer() 
        .requestHandler(router::accept) 
        .listen(8080);
}
```

So, we can add more handlers and routes:

```java
router.get("/byebye/") 
        .handler(req -> req.response()
            .putHeader("content-type", "text/html") 
            .end("<html><body><h1>Bye Bye World</h1></body></html>"));
```

A vertical can be deployed using the `deployVerticle` method.

```java
public static void main(String[] args){
   Vertx vertx = Vertx.vertx();
   vertx.deployVerticle(new HelloWorldVerticle(), res -> {
      if (res.succeeded()) {
         log.info("Deployment id is: " + res.result());
      } else {
         log.info("Deployment failed!");
      }
   });
}
```

Vert.x offers an elegant way to avoid bottlenecks in many concurrent connections used by a solid foundation for the asynchronous paradigm. In this way, it lets your app scale with minimal hardware. Also, it is lightweight and flexible.