---
layout: post
title: Apache Kafka - Introduction
tags: kafka
---

Kafka is gaining so much traction and hiring managers are looking for developers with this skill. Why? Because asynchronous communication between microservices can help to avoid bottlenecks that monolithic architectures are suffering from. Also, Kafka is a better solution for constructing microservices that require real-time streaming capabilities. Well, I wrote this article to help explain Kafka and present its use cases.

## What Is a Message Queue

The goal of a message queue is to deliver in reliable way communications (or messages). In other words, think of a message queue like a PO box. The postman might deliver a message there any time if the receiver is at home or nor and can keep putting the message there. The receiver can retrieve the message immediately or lately.

In this example, the postman is the **"Publisher"** or **"Producer"** and when the receiver who goes and gets the mail is the **"Subscriber"** or **"Consumer"**.

## Why Use a Message Queue?

Today's applications are responsible for handling huge volumes of data for thousands of users connected at the same time. Thus complex operations of non-blocking flows with fault tolerance are inevitable. On the other hand, the nature of the user experience demands fast applications. So asynchronous data exchange is the solution for large scale applications.

## What is Kafka?

Kafka is a messaging system enabling communication between producers and consumers using message based topics. Wait, but why this introduction about Message Queues? Well, the chief difference with Kafka is storage, it saves data using a commit log but Kafka is a fast, scalable, fault-tolerant, publish-subscribe messaging system. One of the best features of Kafka is, it is highly available and resilient to node failures and supports automatic recovery. Moreover, this system replaces the conventional message queues, with the ability to give higher throughput, reliability, and replication like JMS, AMQP, etc. Also as we said before, Kafka stores the messages in Topics. Consumers can "replay" these messages if they wish. Normally in message queues, the messages are removed after subscribers have confirmed their receipt. Another thing different about Kafka is that the topics are ordered (by the date they were added). Not all message queues guarantee this. Wait wait, there are more concepts here to remember. 

## Glossary

Below is the list of most prominent Kafka terminologies which may help you to build the strong foundation of Kafka knowledge:

- Broker: a Kafka cluster is, basically, a set of individual servers. If Kafka has more than one broker, that is what we call a Kafka cluster
- Topic: Kafka messages are generally organized into named lists called topics
- Partition: In a broker, there is some number of partitions. These partitions can be both a leader or a replica of a topic. So, on defining a Leader, it is responsible for all writes and reads to a topic whereas if somehow the leader fails, replica takes over as the new leader.
- Producer: the processes which publish messages
- Consumer: the processes that subscribe to topics and process as well as read the feed of published messages
- Offset: the position of the consumer in the log and which is retained on a per-consumer basis is what we call Offset
- Consumer Group: with a consumer group name, Consumers can label themselves
- Node: a single computer
- Cluster: a group of computers, each having one instance of Kafka broker
- Replicas: a replica of a partition is a “backup” of a partition.
- Message: a piece of information which travels from the producer to a consumer through Apache Kafka
- Leader: A node that is responsible for all reads and writes for the given partition is what we call a Kafka Leader. So, every partition consists of one server, which acts as a leader
- Follower: a node that follows leader instructions is what we call a follower
- Data Log: Messages are preserved through Kafka, especially for a considerable amount of time
- Connector API: The API which permits to build as well as run reusable consumers or producers that connect existing applications or data systems to Kafka topics, we use the Connector API. 

So, this was all about Apache Kafka Terminologies. Hope you like our explanation.

## Messaging System Types

- Point to Point Messaging System: message can be consumed by a maximum of one consumer only. The message disappears after is marked red. 
- Publish-Subscribe Messaging System: consumers can subscribe to one or more topics and consume all the messages on that topic.

## History of Apache Kafka

Kafka was originally developed at LinkedIn to handle large quantities of traffic and provide a platform for handling real-time data feeds. Then, in the year 2011 Kafka was made public.

## Architecture

Kafka Architecture is based on four core APIs:

1. Kafka Producer API

   This Kafka Producer API permits an application to publish a stream of records to one or more Kafka topics.

2. Kafka Consumer API

   To subscribe to one or more topics and process the stream of records produced to them in an application, we use this Kafka Consumer API.

3. Kafka Streams AP

  To act as a stream processor consuming an input stream from one or more topics and producing an output stream to one or more output topics and also effectively transforming the input streams to output streams, this Kafka Streams API permits an application.

4. Kafka Connector API

   This Kafka Connector API allows building and running reusable producers or consumers that connect Kafka topics to existing applications or data systems. For example, a connector to a relational database might capture every change to a table.

## Java in Kafka

Apache Kafka is written in pure Java and also Kafka’s native API in java.

## Kafka Use Cases

Several use cases of Kafka show why we use Apache Kafka:

- Messaging, async communication in microservices architectures: fault-tolerance which makes it a good solution for large-scale message processing applications
- Metrics: it includes aggregating statistics from distributed applications to produce centralized feeds of operational data
- Event Sourcing

## Comparisons in Kafka

- RabbitMQ vs Apache Kafka: Kafka is distributed, guaranteed durability and availability, the data is shared and replicated, Kafka 5x times faster, distributed processing.

## Conclusion

Kafka is being adopted in many large organizations because of the ability to store data messages indefinitely and deal with high amounts of traffic.