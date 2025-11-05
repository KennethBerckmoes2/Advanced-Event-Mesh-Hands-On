# Hands-On: SAP Integration Suite, advanced event mesh


## SAP Integration Suite, advanced event mesh

SAP Integration Suite, advanced event mesh is a fully managed event streaming and management service that enables enterprise-wide and enterprise-grade event-driven architecture. It complements SAP Event Mesh for more demanding scenarios and is a full blown, general purpose Event Mesh.

AEM offers enterprise-grade performance, reliability, security and governance. It scales to very large use cases – and very means very very very in this case.

![Pic 2](/./images/ex0_002.png)

SAP Integration Suite, advanced event mesh offers deployment options across different hyperscalers and in private cloud environments. It can be configured to form a distributed mesh of event brokers deployed across environments in private or public clouds. A full purpose set of eventing services including event streaming, event management and monitoring is provided and on top advanced features like dynamic message routing and fine-grained filtering.

## Description of the hands-on session

Explore SAP Integration Suite, advanced event mesh and some of its main features. 

Learn how you can improve your situational awareness by having your SAP backends, namely SAP S/4HANA Cloud and SAP SuccessFactor solutions, act as a real-time teamplayer.

Consume events from SAP S/4HANA and SAP SuccessFactors solutions via Advanced Event Mesh in a basic extension application.

See the advantages of a mesh of event brokers over a single event broker.

## Overview

This session allows you to get an end-to-end impression of event-driven architecture hands-on. Starting from the event source, you will follow the flow of outbound events via the event broker to the event consumer.

As an event source we will use SAP S/4HANA Cloud and SAP SuccessFactors. SAP Integration Suite, advanced event mesh will be our event broker, and events are consumed by an HTML/JS based event consumer.

The session will be broad rather than deep, with the goal to provide a highspeed end-to-end experience, with a specific focus on our new event broker SAP Integration Suite, advanced event mesh. 

You will be able to experience basic and advanced features SAP Integration Suite, advanced event mesh.

During the free flow part towards the end you can use the HTML/JS based event consumer and an HTML/JS based event producer to try out different features of SAP Integration Suite, advanced event mesh.

![Pic 3](/./images/ex0_003.png)

## Features touched, some just shortly, some in more detail, include:

* SAP S/4HANA Cloud standard notification events
* SAP SuccessFactors Solutions events
* SAP Business Accelerator Hub
* SAP Integration Suite, advanced event mesh queues
* SAP Integration Suite, advanced event mesh Cluster Manager
* SAP Integration Suite, advanced event mesh Mesh Manager
* SAP Integration Suite, advanced event mesh Event Insights
* SAP Integration Suite, advanced event mesh Event Designer and Event Management
* SAP Integration Suite, advanced event mesh Try Me!
* SAP Integration Suite, advanced event mesh Event Portal
* SAP Integration Suite, Cloud Integration

## Requirements

- Understanding the basics of event-driven architectures, namely events, queues, topics, event subscriptions ...
- You would be able to execute most of the excercises without prior experience by just following the descriptions. For taking value out of the chance to explore the brokers, specifically Advanced Event Mesh, some experience with event-driven architectures is recommended.

## Background Material

A lot of material to get up to speed with SAP Integration Suite, advanced event mesh is available.

- Learning
  
    - [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
    - [Discovering Event Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/learning-journeys/discovering-event-driven-integration-with-sap-integration-suite-advanced-event-mesh)
    - [SAP Integration Suite, Advanced Event Mesh Blog Collection](https://community.sap.com/t5/technology-blog-posts-by-sap/sap-integration-suite-advanced-event-mesh-blog-collection/ba-p/14111943)

- Documentation

    - [Help](https://help.pubsub.em.services.cloud.sap/Cloud/cloud-lp.htm)


## Exercises

In the following, the complete list of exercise steps are listed. Run through them in the given order. You can use this section as an index or table of contents. Use the breadcrumb navigation on top of the pages to go back to the Table of Contents. Exercises marked as (optional) can be skipped if wanted.

Preparation and Setup

- [Getting Started](exercises/ex0/) (5 mins)

SAP Business Accelerator Hub (optional)

- [Exercise 1 - Explore SAP S/4HANA Cloud Standard Events in SAP Business Accelerator Hub](exercises/ex1/) (15 mins)

    - [Exercise 1.1 - Look up BusinessPartner events in SAP SAP Business Accelerator Hub](exercises/ex1/)

Discovering SAP Integration Suite, advanced event mesh 
    
- [Exercise 2 - First Steps in Advanced Event Mesh](exercises/ex2/) (30 mins)

    - [Exercise 2.1 - Log into Advanced Event Mesh and explore it](exercises/ex2#exercise-21---log-into-advanced-event-mesh-and-explore-it)
    - [Exercise 2.2 - Create a queue in Advanced Event Mesh](exercises/ex2#exercise-22---create-a-queue-in-advanced-event-mesh)
    - [Exercise 2.3 - Create a Queue Subscription for S/4HANA Events](exercises/ex2#exercise-23---create-a-queue-subscription-for-sap-s4hana-events-in-advanced-event-mesh)
 
Consuming from a Queue in SAP Integration Suite, advanced event mesh 
 
- [Exercise 3 - Consume SAP S/4HANA Event from Advanced Event Mesh Queue](exercises/ex3/) (5 mins)

    - [Exercise 3.1 - Start your Queue Consumer Application and connect to Advanced Event Mesh](exercises/ex3#exercise-31-start-your-queue-consumer-application-and-connect-to-advanced-event-mesh)
    - [Exercise 3.2 - Consume and explore events in Queue Consumer](exercises/ex3/README.md#exercise-32-consume-and-explore-events-in-queue-consumer)

Consuming from a Topic in SAP Integration Suite, advanced event mesh 

- [Exercise 4 - Consume SAP S/4HANA Event via Advanced Event Mesh Topic](exercises/ex4/) (10 mins)
 
    - [Exercise 4.1 - Mesh Manager](exercises/ex4/README.md#exercise-41-mesh-manager)
    - [Exercise 4.2 - Use TryMe! to consume events from a topic on a second broker](exercises/ex4#exercise-42-consume-events-via-a-topic-on-a-second-broker)
    - [Exercise 4.2 - Produce and Consume events via a topic](exercises/ex4#exercise-42-produce-and-consume-events-via-a-topic)
      
- [Exercise 5 - Explore Topic Hierarchies and Wildcards](exercises/ex5/) (15 mins)
 
    - [Exercise 5.1 - Learn about Topic Hierarchies and Wildcards](exercises/ex5#exercise-51-learn-about-topic-hierarchies-and-wildcards)
    - [Exercise 5.2 - Practice Topic Hierarchies and Wildcards using Try Me !](exercises/ex5#exercise-52-practice-topic-hierarchies-and-wildcards-using-try-me----animal-edition)   
   
SAP Integration Suite, advanced event mesh additional features      
   
- [Exercise 6 - Advanced Event Mesh as an end-to-end platform for Event-Driven Architectures](exercises/ex6/) (20 mins)

    - [Exercise 6.1 - Explore Insights](exercises/ex6#exercise-61-explore-insights)
    - [Exercise 6.2 - Event Management](exercises/ex6/README.md#exercise-62-event-management)
 
The power of SAP Integration Suite & SAP Integration Suite, advanced event mesh  
   
- [Exercise 7 - Building a micro-integration in SAP Integration Suite](exercises/ex7/) (1h30)

    - [Exercise 7.1 - Prepare and deploy your micro-integration flow](exercises/ex7/README.md#exercise-71-prepare-and-deploy-your-micro-integration-flow)
    - [Exercise 7.2 - Adding converters to the micro-integration flow](exercises/ex7/README.md#exercise-72-adding-converters-to-the-micro-integration-flow)
    - [Exercise 7.3 - Adding a message mapping to the micro-integration flow](exercises/ex7/README.md#exercise-72-adding-converters-to-the-micro-integration-flow)
    - [Exercise 7.4 - Publishing the event as a CSV on an SFTP server](exercises/ex7/README.md#exercise-72-adding-converters-to-the-micro-integration-flow)


## License
Copyright (c) 2022 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
