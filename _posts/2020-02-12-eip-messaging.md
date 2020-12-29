---
layout: posts
title:  "Enterprise Integration Pattern, Async Messaging"
date:   2020-12-12 10:47:34 -0600
categories: messaging
---
Applications cannot exist in isolation. For instance, a typical accounting application manages its content using document management service, receives data streams from e-commerce application, and sends  notifications to registered user. To integrate these services you can use one of these common patterns; shared databases, exchanging files, making remote calls, or messaging.

With shared database pattern all application functions have full access to the database and a function can read or change data written by another function to perform the integration task. This is an easy pattern to start with  but as the application complexity increases the rate at which the application can evolve degrades over the long term. Application can become a melting pot of logic and data. Increasing complexity and chances of regressions.

Exchanging data file pattern is used by applications that move large amounts of data periodically. For instance, your application downloads transactions nightly from your banks and adds each transaction  to the ledger, or, your application logs are sent periodically to an application (like Splunk) that makes log analysis easier. This pattern is not synchronous making it unusable for many applications, and the circle of such application is growing because of fast changing user expectations.

Making remote calls is a widely adopted pattern. Establishing a point to point communication is quick to get off the ground and does not require a special tools. The simplest example is a REST service called by an HTTP client to perform a function. When you visit a  site looking for movies playing near you, most likely a remote call-based system is fulfilling that request. Some of the largest commercial applications on the internet use this integration pattern. But it not without some drawbacks; lends to a tightly coupled system because the underlying service can evolve  but the contracts may be locked. Newer services in the application need to speak the language of legacy services. Increased application complexity if guaranteed processing of all incoming requests is a requirement. 

Async messaging based integration is the most reliable of the three patterns. Integrating applications send messages to each other which are processed asynchronously. Messaging technology has evolved to support both a large number of messages and provide sub-second response times rivaling direct calls.

With direct calls as the standard lets examine how async messaging patterns meets these expectations. 

Async messaging system supports two patterns for passing messages between services.

*Point-to-point messaging*: A point-to-point channel is established between message publisher and a subscriber capable of processing the message. To establish a point-to-point channel create a queue with two endpoints, one each for publishing and subscribing messages. Messages are processed in the order they arrive.

*Publish/subscribe*: When multiple parties are interested in the same message, messages are published to a topic and every subscriber receives a copy of the message. For instance, many applications may be interested in the price of a stock listed on an exchange.

To scale message processing add multiple subscribers. Messages are delivered in a round-robin to the group of subscribers also known as competing consumers. Message order is lost in this setup.

Messaging systems provide additional capabilities that help scale integrations and simplify

*Message filters*: Point to point channel architecture can cause an operational challenge causing an explosion of channels. For instance, anytime a new type of message arrives new channels need to be established. Message filters can alleviate this pain. Subscribers sit behind a filter which only allows messages that subscriber is interested in. New message types would not impact existing subscribers.

*Message router*: Managing message filters spread across the network can be challenging, especially when you are making a change that affects multiple filters, a message router can help centralize this logic. All filtering logic lives in the message router, the router has a channel for inbound messages and multiple outbound channels. A subscriber would connect with the outbound channels.

*Canonical models and transformers*: Message filters and router simplify point to point channels, but the message format expected by each service in the system can present another challenge. A canonical data model adds a level of indirection between service formats. Messages inside the pipeline flow in a standard model and consuming services covert message to the format they understand. 
A message transformer (also know as adapters) transforms messages from the canonical data model to a service-specific format, like a spoken language interpreter. The canonical model of the application can evolve without affecting connecting services. This is especially helpful when connecting legacy service into a messaging channel


A *message broker* is an architectural pattern that can bring all these capabilities together. A broker mediates communication between services. It can be the central component that receives all messages, transforms them as necessary, and hands them over to a message router to deliver them to the right channel. 
A single instance of the broker operates in a hub and spoke model. All inbound messages arriving at the hub and distributed out to the subscribers. This model has a single point of failure and can be inefficient if the publisher and subscribers are geographically distributed. In such cases, multiple brokers may be to configured, ideally one for each cluster of co-located subscribers. Subscribers depending on their needs can connect to the closest broker and un-processed messages travel from broker to broker.

*Kafka* is a distributed message broker that supports a publish/subscribe integration pattern. Kafka provides scale and reliability by operating on a cluster of machines. Producers and consumers are the basic types of users of the system. Producers write messages to topics that are divided into multiple partitions and the partitions are distributed across the cluster. Multiple copies of the same partitions are kept to improve the reliability of the system. This system of partitions also provides a scale where Kafka can be used as a stream processor like Samza and Apache storm.

Kafka cluster can be configured for high consistency or high availability. High consistency is achieved by making sure  writes are successfully replicated across multiple partitions. Availability can be increased by increasing the number of partitions, even if one machine goes down consumer will be redirected to other partitions. Apache zookeeper manages information about the partitions and the role they play in the cluster.

Reference: 

[Reactive streams](https://blog.redelastic.com/a-journey-into-reactive-streams-5ee2a9cd7e29#.2wqcc3cja)
[ActiveMQ Topologies](https://activemq.apache.org/how-do-distributed-queues-work.html)
