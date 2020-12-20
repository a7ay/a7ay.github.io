---
layout: posts
title:  "Enterprise Integration Pattern, Async Messaging"
date:   2020-12-12 10:47:34 -0600
categories: messaging
---
Applications cannot exist in isolation. Your accounting application has links to documents managed by document service, receives data streams from your e-commerce application, and sends out notifications. The four common patterns of integration between Applications; shared databases, exchanging files, making remote calls, or messaging.

A shared database pattern is used when integrating applications that are co-located.

Exchanging data files is common among applications that move large amounts of data periodically. For instance, your application downloads a file of transactions nightly from your banks and add them to your balance sheet. Your application's logs are sent periodically to an application (like Splunk) that makes log analysis easier.

Making remote calls is a widely adopted pattern. Establishing a point to point communication is quick to get off the ground and does not require a special toolchain. The simplest example is a REST service called by an HTTP client to perform a function. When you visit a movie site looking for what is playing near you, most likely a remote call-based system is fulfilling that request. Some of the largest commercial applications on the internet use this integration pattern. But it not without some of its drawbacks; lends to a tightly coupled system, services can evolve independently but the contracts may be locked. Many point to point connections make upgrades difficult. Newer applications need to speak the language of legacy systems that are part of the application. Application complexity increases if it needs to support guaranteed processing. The application needs to manage all the metadata describing the service, its location and connections.

The final pattern of Async messaging is the most reliable way to integrate applications. Integrating applications send messages to each other which are processed asynchronously. Messaging technology has evolved to support both a large number of messages and provide sub-second response time usually expected from direct calls.

A messaging system supports two patterns.

Point-to-point messaging: The simplest messaging system has a point-to-point channel in which a publisher writes messages to an endpoint and the subscriber on the other end of the channel will retrieve the message for processing and share the response. To establish a point-to-point channel create a queue with two endpoints, one each for publishing and subscribing messages. Messages are processed in the order they arrive.

Publish/subscribe: When multiple parties are interested in the same message then publish messages to a topic and every subscriber will receive a copy of the message. For instance, many applications may be interested in the price of a stock or the score of a game.

Messaging systems provide additional capabilities that help scale integrations and simplify

Message filters: Point to point channel architecture can cause an operational challenge through the explosion of channels. For instance, anytime a new type of message arrives channel needs to be established with all the subscribers. One way to combat this challenge is to use message filters. All messages are published to the same channel but the subscriber filters the messages they are interested in. New message types would not impact existing subscribers.

Message router: Managing message filters spread across the network can be challenging, especially when you are making a change that affects multiple filters, a message router can help centralize this logic. All filtering logic lives in the message router, the router has a channel for inbound messages and multiple outbound channels. A subscriber would connect with the outbound channels.

Message filters and router help manage point to point channels, but the language spoken by the different services in a system can present another challenge. A canonical data model adds a level of indirection between service formats. Message inside the pipeline flow in a standard model and consuming services covert message to their format. This responsibility can be given to another component of the message system, the message transformer.

A message transformer (also know as adapters) transforms messages from the canonical data model to a service-specific format, like a spoken language interpreter. The canonical model of the application can change without affecting any of the services. This is especially helpful when connecting legacy service into a messaging channel

To scale message processing a point of point channel can have multiple subscribers. Messages are delivered in a round-robin to the competing consumers. This may not work for all cases where message order is a requirement.

A message broker is an architectural pattern that can bring all these capabilities together. A broker mediates communication between applications. It can be the central component that receives all messages, transforms them as necessary, and hands them over to a message router for delivering them to the right channel. A single instance of the broker operates in a hub and spoke model. All inbound messages arriving at the hub and distributed out to the subscribers. This model has a single point of failure and can be inefficient if the publisher and subscribers are geographically distributed. In such cases, a better approach may be to configure several message brokers, ideally one for each cluster of co-located subscribers. Subscribers depending on their needs can connect to the closest broker and un-processed messages travel from broker to broker. Multiple brokers can increase chattiness in the network with messages crisscrossing brokers, but a strategy of prioritizing local brokers over remote brokers can help curb it.

Kafka is a distributed message broker that supports a publish/subscribe style of integration pattern. Kafka provides scale and reliability by operating on a cluster of machines. Producers and consumers are the basic types of users of the system. Producers write messages to topics that are divided into multiple partitions and the partitions are distributed across the cluster. Multiple copies of the same partitions are kept to improve the reliability of the system. This system of partitions also provides a scale where Kafka can be used as a stream processor like Samza and Apache storm.

The cluster can be configured for high consistency or high availability. High consistency can be achieved by making sure each write was successfully replicated across multiple partitions. This may slow down the rate at which new messages can be written. Availability can be increased by managing the number of partitions, even if one machine goes down consumer will be redirected to other partitions. Apache zookeeper manages information about all the partitions and the role they play in the cluster.

Reference: 
[Reactive streams](https://blog.redelastic.com/a-journey-into-reactive-streams-5ee2a9cd7e29#.2wqcc3cja)
[ActiveMQ Topologies](https://activemq.apache.org/how-do-distributed-queues-work.html)
