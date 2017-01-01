---
layout: post
title: Asynchronous communication in Microservices
tags: microservices architecture
---

Microservices are not data silos but they will often make calls to other pieces of the system. Though we can have synchronous calls when the requester expects an immediate response, integrating asynchronous communication based on events can provide more resiliency. To build scalable systems, we need event-driven communications between microservices.



https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/communication-in-microservice-architecture





There are a lot of options for asynchronous communications . Some of the widely used ones are:

- Kafka
- RabbitMQ
- Google Pub/Sub
- Amazon Services
- ActiveMQ
- Azure Services

## **Communication Types**

### Message Queuing

In Message Queuing system, messages are persisted inside a queue. One or more consumers can consume the messages in the queue. In some cases, a message can be configured to be consumed once by only one consumer. The message is persisted in the queue waiting for its consumer.

### Publish subscribe

In the publish-subscribe system, messages are persisted in a topic. Consumers can subscribe to one or more topics and consume all the messages in that topic. In the Publish-Subscribe system, message producers are called publishers and message consumers are called subscribers.

## **Implementations**

### Kafka:

Kafka is the most popular open source distributed publish-subscribe streaming platform that can handle millions of messages per minute. The key capabilities of Kafka are:

- Publish and subscribe to streams of records
- Store streams of records in a fault tolerant way
- Process streams of records as they occur

Those features make Kafka a natural choice for a number of use cases which are listed [here](https://kafka.apache.org/uses)

The Key Components of Kafka architecture are: Topics, Partitions, Brokers, Producer, Consumer, Zookeeper.

Core APIs in Kafka include:

- Producer API allows an application to publish a stream of records to one or more Kafka topics.
- Consumer API allows an application to subscribe to one or more topics and process the stream of records produced to them.
- Streams API allows applications to act as a stream processor, consuming an input stream from one or more topics and producing an output stream to one or more output topics, effectively transforming the input streams to output streams. It has a very low barrier to entry, easy operationalization, and a high-level DSL for writing stream processing applications. As such it is the most convenient yet scalable option to process and analyze data that is backed by Kafka.
- Connect API is a component that you can use to stream data between Kafka and other data systems in a scalable and reliable way. It makes it simple to configure connectors to move data into and out of Kafka. Kafka Connect can ingest entire databases or collect metrics from all your application servers into Kafka topics, making the data available for stream processing. Connectors can also deliver data from Kafka topics into secondary indexes like Elasticsearch or into batch systems such as Hadoop for offline analysis.

More details [here](https://kafka.apache.org/intro)

Kafka is written in Scala and Java. It was originally developed by LinkedIn and donated to the Apache Foundation.

#### Kafka as a Messaging System:

![Image for post](https://miro.medium.com/max/60/1*KI_6sbPX3ZkNC3b52bF22g.png?q=20)

![Image for post](https://miro.medium.com/max/796/1*KI_6sbPX3ZkNC3b52bF22g.png)

#### Kafka architecture

![Image for post](https://miro.medium.com/max/60/1*HPDpDnVT7xAKHI7Jn4e6BQ.png?q=20)

![Image for post](https://miro.medium.com/max/1200/1*HPDpDnVT7xAKHI7Jn4e6BQ.png)

Kafka Broker with Topics and Partitions

Kafka is a distributed, replicated commit log. Kafka does not have the concept of a queue which might seem strange at first, given that it is primary used as a messaging system. Queues have been synonymous with messaging systems for a long time. Let’s break down “distributed, replicated commit log” a bit:

- Distributed because Kafka is deployed as a cluster of nodes, for both fault tolerance and scale
- Replicated because messages are usually replicated across multiple nodes (servers).
- Commit Log because messages are stored in partitioned, append only logs which are called Topics. This concept of a log is the principal killer feature of Kafka.

Kafka uses a pull model. Consumers request batches of messages from a specific offset. Kafka permits long-pooling, which prevents tight loops when there is no message past the offset. A pull model is logical for Kafka because of its partitions. Kafka provides message order in a partition with no contending consumers. This allows users to leverage the batching of messages for effective message delivery and higher throughput.

It is a dumb broker / smart consumer model — does not try to track which messages are read by consumers. Kafka keeps all messages for a set period of time.

### RabbitMQ:

The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Actually, quite often the producer doesn’t even know if a message will be delivered to any queue at all.

Instead, the producer can only send messages to an exchange. An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives. Should it be appended to a particular queue? Should it be appended to many queues? Or should it get discarded. The rules for that are defined by the exchange type (direct, topic, headers and fanout)

![Image for post](https://miro.medium.com/max/60/1*HoAG-7IhLaXShPJG-g9kvA.png?q=20)

![Image for post](https://miro.medium.com/max/1206/1*HoAG-7IhLaXShPJG-g9kvA.png)

#### RabbitMQ architecture

The super simplified overview:

- Publishers send messages to exchanges
- Exchanges route messages to queues and other exchanges
- RabbitMQ sends acknowledgements to publishers on message receipt
- Consumers maintain persistent TCP connections with RabbitMQ and declare which queue(s) they consume
- RabbitMQ pushes messages to consumers
- Consumers send acknowledgements of success/failure
- Messages are removed from queues once consumed successfully

It is a smart broker / dumb consumer model — consistent delivery of messages to consumers, at around the same speed as the broker monitors the consumer state.

RabbitMQ uses a push model. Push-based systems can overwhelm consumers if messages arrive at the queue faster than the consumers can process them. So to avoid this each consumer can configure a prefetch limit (also known as a QoS limit). This basically is the number of unacknowledged messages that a consumer can have at any one time. This acts as a safety cut-off switch for when the consumer starts to fall behind.This can be used for low latency messaging.

The aim of the push model is to distribute messages individually and quickly, to ensure that work is parallelized evenly and that messages are processed approximately in the order in which they arrived in the queue.

RabbitMQ is written in Erlang. Pivotal develops and maintains RabbitMQ.

### Google cloud Pub/Sub:

Cloud Pub/Sub is a fully-managed real-time messaging service that allows you to send and receive messages between independent applications. You can leverage Cloud Pub/Sub’s flexibility to decouple systems and components hosted on Google Cloud Platform or elsewhere.

Here is the overview of the flow in cloud Pub/Sub system:

- A publisher application creates a topic in the Cloud Pub/Sub service and sends messages to the topic. A message contains a payload and optional attributes that describe the payload content.
- The service ensures that published messages are retained on behalf of subscriptions. A published message is retained for a subscription until it is acknowledged by any subscriber consuming messages from that subscription.
- Cloud Pub/Sub forwards messages from a topic to all of its subscriptions, individually. Each subscription receives messages either by Cloud Pub/Sub pushing them to the subscriber’s chosen endpoint, or by the subscriber pulling them from the service.
- The subscriber receives pending messages from its subscription and acknowledges each one to the Cloud Pub/Sub service.
- When a message is acknowledged by the subscriber, it is removed from the subscription’s message queue.

![Image for post](https://miro.medium.com/max/60/0*lHojUW99N15hlwuN?q=20)

![Image for post](https://miro.medium.com/max/1312/0*lHojUW99N15hlwuN)

#### Google Pub/Sub architecture

Publishers can be any application that can make HTTPS requests to googleapis.com: an App Engine app, a web service hosted on Google Compute Engine or any other third-party network, an installed app for desktop or mobile device, or even a browser.

Pull subscribers can also be any application that can make HTTPS requests to googleapis.com. Push subscribers must be Webhook endpoints that can accept POST requests over HTTPS.

More details [here](https://cloud.google.com/pubsub/)

### Amazon Services:

**Amazon MQ** is a managed message broker service for Apache ActiveMQ that makes it easy to set up and operate message brokers in the cloud.

**Amazon Simple Queue Service (SQS)** is Amazon’s cloud-based message queuing system made available as part of Amazon Web Services (AWS). Unlike the other brokers mentioned here, there is barely any setup/deployment required for an application to use SQS. All you need is AWS credentials to be able to use it. Since it is a SaaS (Software-as-a-service), this makes it a much lower-cost option since there is no infrastructure cost. You only pay for what you use. SQS also has the notion of in-flight messages. This means that the message is pulled off the queue but never deleted until the ‘delete‘ command is sent for that message ID. So if you lose your worker mid-processing of the event, that event isn’t lost for good.

One drawback of the SQS implementation is the need for polling of a message queue to determine if new messages have appeared. This is a bit of an issue being as you must now model your application to perform a polling cycle in order to determine if new messages are available.

In other words, it offers serverless queues — you don’t have to pay for the infrastructure, just the messages you send and receive.

**Amazon Simple Notification Service (Amazon SNS)** is a web service that makes it easy to set up, operate, and send notifications from the cloud. Amazon SNS follows the “publish-subscribe” (pub-sub) messaging paradigm, with notifications being delivered to clients using a “push” mechanism that eliminates the need to periodically check or “poll” for new information and updates. With simple APIs requiring minimal up-front development effort, no maintenance or management overhead and pay-as-you-go pricing, Amazon SNS gives developers an easy mechanism to incorporate a powerful notification system with their applications.

It is comparable to serverless topics. It will notify your services when a message arrives, but if you’re offline you can miss it. SNS can feed into SQS, so if you have some service that may be up and down, you can guarantee it gets SNS messages by queuing them in SQS for it to consume on its schedule.

### ActiveMQ:

ActiveMQ is a Java-based open source project developed by the Apache Software Foundation. ActiveMQ makes use of the Java Message Service (JMS) API, which defines a standard for software to use in creating, sending, and receiving messages.

ActiveMQ sends messages between client applications — producers, which create messages and submit them for delivery, and consumers, which receive and process messages. The ActiveMQ broker routes each message through one of two types of destinations:

A queue, where it awaits delivery to a single consumer (in a messaging domain called point-to-point), or a topic, to be delivered to multiple consumers that are subscribed to that topic (in a messaging domain called publish/subscribe, or “pub/sub”)

ActiveMQ gives you the flexibility to send messages through both queues and topics using a single broker. In point-to-point messaging, the broker acts as a load balancer by routing each message from the queue to one of the available consumers in a round-robin pattern. When you use pub/sub messaging, the broker delivers each message to every consumer that is subscribed to the topic.

![Image for post](https://miro.medium.com/max/44/0*wNdSf2AUaJhTXjqt?q=20)

![Image for post](https://miro.medium.com/max/1149/0*wNdSf2AUaJhTXjqt)

ActiveMQ architecture

### Azure Services:

**Service Bus** is a brokered messaging system. It stores messages in a “broker” (for example, a queue) until the consuming party is ready to receive the messages.

**Event Grid** uses a publish-subscribe model. Publishers emit events, but have no expectations about which events are handled. Subscribers decide which events they want to handle.

**Event Hub** is an event ingestor capable of receiving and processing millions of events per second. Producers send events to an event hub via AMQP or HTTPS. Event Hubs also have the concept of partitions to enable specific consumers to receive a subset of the stream. Consumers connect via AMQP.

More details [here](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)

For an extensive list of queueing systems check [here](http://queues.io/)

## **When to use What**

![Image for post](https://miro.medium.com/max/60/1*Rp0pj-6uOFq4zI-9z_UQfA.png?q=20)

![Image for post](https://miro.medium.com/max/1276/1*Rp0pj-6uOFq4zI-9z_UQfA.png)

Prefer Kafka — When your application needs highly scalable messaging, able to receive a very high number of events, coming from different sources, and delivering them to different clients over a common hub. Message consumers can consume the data as they wish, and re-read and replay messages on demand. For this use case, Apache Kafka’s ability to scale to hundreds of thousands of events per second, delivered in partitioned order, for a mix of online and batch clients is the best fit.

Prefer Kafka if you need your messages to be persisted as well.

Prefer RabbitMQ — When you need a finer-grained consistency control/guarantees on a per-message basis (dead letter queues, etc.). When your application needs variety in point to point and publish/subscribe messaging. When you have complex routing to consumers and integrating multiple services/apps with non-trivial routing logic.

Hopefully this blog post will help you choose the technology that is right for you and if you have chosen one recently, do let me know on what reason you chose it over the others.













In almost all cases Microservice requires a communication protocol. When it is designed, it is required to remember that other services will need to integrate with it. The Microservices definitions do not indicate what technology or what style of communication should be used. In practice you need to find the best solutions for the problems you are solving.

In the following blogpost we’ll discuss different approaches and technologies often used in Microservices world, analyzing their pros and cons.

> Take a look also on my other blogpost “[Are you sure you’re using microservices?](https://blog.softwaremill.com/are-you-sure-youre-using-microservices-f8d4e912d014)”

# Web Services — SOAP

> A Web service is a software system designed to support interoperable machine-to-machine interaction over a network. It has an interface described in a machine-processable format (specifically WSDL). Other systems interact with the Web service in a manner prescribed by its description using SOAP-messages, typically conveyed using HTTP with an XML serialization in conjunction with other Web-related standards.
>
> [W3C, Web Services Glossary](https://www.w3.org/TR/2004/NOTE-ws-gloss-20040211/#webservice)

Probably you have heard before about SOAP and WSDL in SOA (Service Oriented Architecture). But isn’t SOA Microservices? All those technologies are usually recognized as “Enterprise” approach. They include a lot of XMLs and schemas defining how communication protocols look like. There are also a lot of related standards and specifications. Just take a look at this [huge list](https://en.wikipedia.org/wiki/List_of_web_service_specifications)! Would I consider leveraging SOAP for brand new Microservice? Probably not. Usually it’s overcomplicated, but has its applications. There are companies using [ESBs ](https://en.wikipedia.org/wiki/Enterprise_service_bus)(Enterprise Service Bus) where communication usually happens using SOAP, but not always.

It’s worth to remember that SOAP exists, it has its applications, but probably you won’t use it for a brand new simple Microservice.

**Short info:**

- Often XMLs over HTTP, but supports many protocols

**Pros:**

- Very powerful
- Requests and responses have a predefined schema

**Cons:**

- Overcomplicated, e.g. to transfer a file you may need to base64 [encode](https://en.wikipedia.org/wiki/Message_Transmission_Optimization_Mechanism) it and include in the XML
- Less and less popular

**Example:**

```
POST /InStock HTTP/1.1
Host: www.example.org
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 299
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"

<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="http://www.example.org">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <m:GetStockPrice>
      <m:StockName>GOOG</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
```

Source: https://en.wikipedia.org/wiki/SOAP

# REST (REpresentational State Transfer)

Nowadays almost everybody is using REST everywhere (or is claiming their JSON over HTTP is REST). It became an industry standard. Often people even don’t think or discuss, just use REST with JSON as a default.

REST was defined by Roy Fielding in 2000 in his PhD dissertation “Architectural Styles and the Design of Network-based Software Architectures”. In the following years, the number of web APIs thrived.

> If you’d like to learn more about REST history, check the “[The History of REST APIs](https://blog.readme.io/the-history-of-rest-apis/)” blogpost.

REST generally provides a concept of resources, represented by HTTP URIs, where standard HTTP methods can be used to operate on them. It’s quite simple and provides a very nice learning curve.

However in practice there are many topics which can be realized in multiple ways or are just often discussed in every project:

- API versioning — version presented in the path, query param, custom header or content type?
- GET with body — allowed by HTTP RFCs, but not supported by many HTTP client libraries. When would you like to use it? E.g. for typical complex “search” queries.
- Avoiding verbs — REST should operate on resources, so `/account/close` is rather not fully correct, instead you can use `PATCH` method providing, e.g. `{ "state": "closed" }` JSON. Similarly `/account/search` contains the verb.
- JSON representation of polymorphism, e.g. let’s say that you have a resource which one field may contain array containing two different types. This can be represented as `"field": [{ "type": "TypeA", "field1": ... }]` or `"field": [{ "TypeA" : { "field1": ...}}]` . There is no single answer, and different JSON libraries behaves differently.
- JSON fields casing — camel case or snake case?

Nevertheless, most of those topics can be agreed during a single day.

**Short info:**

- Usually JSON or XML over HTTP (but this can be any other format)
- Leverages HTTP methods and status codes

**Pros:**

- Simple, doesn’t require huge knowledge to start working with it
- Industry “standard” (because everybody is using something a bit differently)
- Requests and responses are usually human readable
- Supported by huge number of libraries (both for client and server)

**Cons:**

- A lot of “not standardized” topics — how to model specific queries, API versioning, “Search” endpoint, etc
- Streaming is often difficult
- No schema for endpoints — people try to fix this using e.g. OpenAPI (Swagger) or RAML.

**Example:**

```
Request:GET http://example.org/stocks/GOOGResponse:{
     "price": "23.50"
}
```

# gRPC

Usually when I start describing gRPC, people think CORBA! [Well, yes, there are some similarities, but gRPC is simpler](https://stackoverflow.com/questions/44452399/what-is-the-difference-between-grpc-and-corba). gRPC, like the name, indicates is a Remote Procedure Call framework. It leverages [Protobuf ](https://developers.google.com/protocol-buffers)(Protocol Buffers) and HTTP/2.

Workflow with gRPC is quite simple, first you need to define `.proto` file defining services, requests and response formats, and then you copy this file to all projects which will communicate with each other. gRPC lib will generated basing on the file the appropriate client or server endpoints. The only thing you need to do is to convert your domain objects to the generated classes.

**Short info:**

- RPC (Remote procedure call)
- Binary (Protobuf) over HTTP/2
- Probuf is used to define endpoints schemas
- Developed and used by Google. Also used e.g. by [Dropbox](https://blogs.dropbox.com/tech/2019/01/courier-dropbox-migration-to-grpc/)

**Pros:**

- Fast (binary!)
- Supported by many languages (you can communicate e.g. Java with Python)
- Supports streaming, both for method parameters and as responses
- Provides generators which based on `.proto` definition generates serializers and deserializers.
- Build in support for API changes —Protobuf schema evolution

**Cons:**

- Less known, higher learning curve due to need to learn Protobuf etc.
- Not human readable, additional tools are needed e.g. to manually test API

**Example** `**.proto**`**:**

```
syntax = "proto3";service Stocks {
   rpc GetStock(GetStockRequest) returns (GetStockResponse){ }
}message GetStockRequest {
  string stock_name = 1;
}message GetStockResponse {
  string price = 1;
}
```

# GraphQL

[GraphQL](https://graphql.org/) is a much newer solution than the ones presented before. Have you ever had a problem in REST where you needed to implement two separate endpoints returning more simple and more complex responses? If yes then GraphQL is a solution to that. From technical perspective usually it’s a single REST endpoint `/grapqhl` taking on input JSON requests with special query DSL. For every query you need to specify, which fields from requested entities should be included in the response.

**Short info:**

- Query language for APIs, but supports [Mutations](https://graphql.org/graphql-js/mutations-and-input-types/)

**Pros:**

- During query it is possible to define which fields should be returned in the response. This allows to query only for data needed for specific view/service.
- Strong typing, resources have predefined schemas
- Supported by many languages
- More and more popular, but…

**Cons:**

- First initial release was in 2015 and a stable one in June 2018, so it’s quite fresh.
- Not all client libraries are already mature
- No HTTP caching support
- Queries always return status code 200

**Example:**

```
Definition:type Stock {
  name: String,
  price: String 
}Query:{
  stock(name: "GOOG") {
    price
  }
}Response:{
  "stock": {
    "price": "23.50"
  }
}
```

This means we’re fetching entity `stocks` by name `GOOG` and the response should include the field `price` .

# Apache Kafka

Kafka is a totally different choice. It’s a message broker, so it embraces asynchronous message-based communication. Instead of making synchronous HTTP request, waiting for the response, in this case we could be just consuming a Kafka topic. Modelling processes with asynchronous communication is more complicated but has its advantages. You don’t depend directly on other services, if they were offline, you can still operate, e.g. post messages to Kafka.

> Reminder: [Reactive Manifesto](https://www.reactivemanifesto.org/) embraces the Message Driven systems. Message passing can ensure loose coupling and isolation.

Kafka supports various message types. It of course supports good old JSONs, but also Avro and Protobuf. What is more, it integrates with Confluence Schema Registry, so you can keep your message schemas in external service.

> Various different message queues were described and benchmarked [here](https://softwaremill.com/mqperf) by Adam Warski.

Kafka can be used also with Even Sourcing, where events are distributed among various topics and dependent services can build current data view from them. You can read more about Event sourcing using Kafka [here](https://blog.softwaremill.com/event-sourcing-using-kafka-53dfd72ad45d).

**Short info:**

- Asynchronous, message based communication

**Pros:**

- Async communication has its big advantages
- Supports different message formats: JSON, [Avro](https://avro.apache.org/), [Protobuf](https://developers.google.com/protocol-buffers), …

**Cons:**

- Modelling application using messages is more complicated
- You need to setup a Kafka cluster

**Example:**

Let’s see how can we model the case of querying specific stock price. First, we would create a `stock_prices` topic. Other services could post messages containing stock prices changes to it. Our service would consume this topic and put the value e.g. in local “cache” (depends on the use case). Message on the topic could be in Avro (but of course could be in JSON, Protobuf or anything else you’d like to).

```
Schema:{
   "namespace": "example.avro",
   "type": "record",
   "name": "Stock",
   "fields": [
      {"name": "name", "type": "string"},
      {"name": "price",  "type": "string"}
   ] 
 }
```

# Conclusion

![Image for post](https://miro.medium.com/max/60/1*jRCIAy5RNpT6-MtN0JGZIw.png?q=20)

![Image for post](https://miro.medium.com/max/461/1*jRCIAy5RNpT6-MtN0JGZIw.png)

There are a lot of options! However you don’t need to make a single choice for communication between all of your services. Every service may choose something else, of course if there are strong reasons for that, not just developer “fun”. Generally, what to choose when?

- If you need to communicate UI (browser) with your service — choose REST or GraphQL
- If you need to provide public API to your service/product — choose REST
- If you need to communicate different internal services — try to model your processes using messages, if not possible then choose gRPC or REST.