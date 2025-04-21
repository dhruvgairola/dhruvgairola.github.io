---
layout: post
title: Microservices From Design to Development
blog: blog
tags: [Technology]
---

I have provided a summary of the book [Microservices : From Design to Development](https://www.nginx.com/resources/library/designing-deploying-microservices/) here.

**Chapter 1 : Introduction**
* Monoliths :
  * Bugs hard to fix.
  * Codebase hard to understand.
  * Long startup time.
  * Continuous deployment impossible.
  * Different modules have conflicting resource requirements.
  * A small bug brings the whole app down.
  * You're stuck with old tech.

* Microservices :
  * Break down monolith to services.
  * Each service is a mini-app with own DB.
  * Modular : Easy to understand, faster to build.
  * Can use different tech for each service.
  * Scale each service independently.
  * Challenges :
    * People may build too many services that are too small.
    * Distributed communication adds complexity.
    * Multiple DBs is a data management challenge. Also, eventual consistency across the different services.
    * Changes that affect multiple services is hard to manage (although this is rare).
    * Deployment is complex.
    * Need service discovery methods.

**Chapter 2 : API Gateways**
* Client (like a mobile app or hardware device) interacts with microservices via API gateway.
* Consider the alternative :
  * Client makes request per microservice.
  * Way too many requests, client code becomes too complex, services may use protocols that are not web-friendly, refactoring services will mean that client code has to be significantly changed, etc.
* API gateway :
  * Handles routing, composition of services, aggregation queries, protocol translation, etc.
  * Each client type can get their own API.
  * Few requests per client compared to above alternative.
  * However, gateway becomes a development bottleneck.
  * Best practices :
    * Use async I/O e.g., Vertx, Node.js
    * Composing requests can result in callback hell. Solution is to use promises, CompletableFutures, etc. These are reactive solutions for API composition.
    * Maintain service registry (a DB of service instances and their locations) for service discovery.
    * Gateway has to handle cases when instances are down. Solution is to use caches, default data, etc.

**Chapter 3 : Inter-process communication**
* Each service is a part of a distributed system so we need an IPC mechanism.
* Types of communication :
  * Synchronous request/response (REST). Is one to one interaction.
  * Request/no response. Is one to one interaction.
  * Request/async response. Is one to one interaction.
  * Async pub/sub. Is one to many interaction.
* Async IPC :
  * Mostly message based. Message consists of headers (metadata) and message body.
  * Advantages : Decouples client from service, buffering of messages, flexible client/service interactions.
  * Disadvantages : Operational and implementation complexity.
* Sync IPC :
  * REST.
  * Apache Thrift: Create client stubs and server side skeletons across languages.
* Message exchange via human readable text formats like XML and JSON or smaller, binary formats like Protocol Buffer and Avro.

**Chapter 4 : Service Discovery**
* A service registry is a DB that contains instances and their locations.
* Client side discovery :
  * Client queries the DB to get locations of services. (By the way, these clients refer to other services, they're not the same clients that were referenced in Chapter 2.)
  * Service has to register and deregister itself to the registry.
  * Advantage : Client can do load balancing.
  * Disadvantage : Client code is coupled with DB query code.
* Server side discovery :
  * Load balancer queries the DB and routes the requests to the correct services. e.g., AWS ELB, Nginx.
  * Advantage : Details of discovery are abstracted from client code.
  * Disadvantage : Setting up load balancer.
* Service registries e.g., Netflix Eureka, Zookeeper, etc.
* Service registration strategies :
  * Do self-registration i.e., services must refresh registration every 30 sec with the registries. This is simple to do but couples the services to the registry.
  * Use third party registration pattern. Here, registrar software handles registration via events. Service publishes events and the registrar updates the registry accordingly. This is great because the services are decoupled from writing registry code. However, you need to setup the registrar software.

**Chapter 5 : Event driven data management**
* Each service has its own DB. Ensures loose coupling. If multiple services shared DBs, then schema updates would require coordinated updates to all the services.
  * Challenges :
    * How to implement transactions that maintain consistency across services?
    * How to implement queries that get data from multiple services?
* Solution is event driven architecture.
  * Service publishes event and other services subscribe to those events and do stuff, and possibly create more events.
  * Can use events to maintain materialized views that pre-join data across services.
  * Advantages : Transactions that span multiple services. Can maintain materialized views.
  * Disadvantages : More complex that ACID. Eventual consistency. Service failures might result is compensating transactions in other services like deleting rows.
  * New Challenge : Atomicity.
* Atomicity :
  * If service crashes after updating DB but before publishing the event, the system becomes inconsistent.
  * Solution 1 : Use local DB
    * Create EVENT table that functions as message queue.
    * A process queries, publishes message and then updates this table.
    * Its simple but devs now have to implement this extra table.
  * Solution 2 : Mining DB transaction log
    * Same as solution 1 except your process queries the DB transaction log file.
    * Better than solution 1 because there is separation of event publishing from app logic but transaction logs are usually different across DBs. Also, you have to parse the logs to determine which updates should create events.
  * Solution 3 : Event sourcing
    * Don't store current app state, store sequence of state changing events.
    * Can reconstruct state by replaying events.
    * Events persisted in event store, which provide APIs for services to subscribe.
    * Good solution because you now have an audit log and no object relational impedance mismatch. But, its a steep learning curve and unfamiliar tech. Plus, you need CQRS to query the event store since event stores only support lookup by primary key.

**Chapter 6 : Deployment of microservices**
* Monolith deployment is N servers and M instances on each server.
* With microservices, you can have tens to hundreds of services, each with variety of frameworks and multiple instances. Deployment must be fast, reliable and cost effective.
* Deployment pattern 1 : Multiple service instances per host
  * Each service runs at a port on one or more host.
  * Each service can be a separate process e.g, WAR file on Tomcat and Node.js running separately on same host.
  * Multiple services may also run on the same process e.g., multiple WAR files on same Tomcat server.
  * Advantages : Efficient usage of host resources, especially if multiple services on same process. Deployment is fast.
  * Disadvantages : No isolation of service instances. Rogue services may use up all resources and starve the others. Also, ops team needs to know how to deploy the service instance.
* Deployment pattern 2 : Service instance per host
  * Each service runs on its own host. 2 varieties exist.
  * Variety 1 : Service instance per VM
    * Create VM image and deploy on VM.
    * Automate VM construction using software like Packer.
    * Advantages : Service instances run in isolation and deployment is simple. Great VMs like EC2.
    * Disadvantages : Less efficient resource utilization. VM might be underutilized. VM images are slow to build and VM are slow to startup.
  * Variety 2 : Service instance per container
    * Containers are virtualization mechanism at the OS level.
    * Create container image first.
    * Run multiple containers per host.
    * Use software like Kubernetes to manage containers. These treat hosts as a pool of resources and decide where to place each container based on the resources required by each container and resources available on each host.
    * Advantages : Same as VMs, but they're more lightweight than VMs, so they're easier to build and startup.
    * Disadvantages : Operational complexity.
* Deployment pattern 3 : Serverless
  * Use AWS lambda. Create a ZIP file and upload to Lambda and this code responds to events and AWS handles all administration.
  * 4 ways to invoke a lambda function :
    * Via REST.
    * Via events from AWS service.
    * Via AWS API Gateway.
    * Via schedule like cron.
  * Advantage : Pay per use. No administration.
  * Disadvantage : Requests must complete within 300 sec so long running services will not work e.g., service that consumes messages. Also, services must be stateless.

**Chapter 7 : Refactoring monolith into microservices**
* Don't refactor everything from scratch. Too risky and most likely will end in failure.
* Refactor strategy 1 : Stop digging
  * Don't add more code to the monolith. Instead, put new code to a standalone service.
  * Need to add glue code between monolith and the new service for data integration.
  * 3 strategies to access monolith data : invoke monolith API, access monolith DB directly or maintain a separate DB that is synced with the monolith DB.
  * Glue code is sometimes called anti-corruption layer because it prevents the service from being polluted by the concepts from the monolith's domain model.
  * Advantages : Service can be deployed and scaled independently of monolith.
  * Disadvantages : Legacy monolith still exists.
* Refactor strategy 2 : Split frontend and backend
  * Separate front end from business logic and DAO layer.
  * However, the 2 layers are still really large.
* Refactor strategy 3 : Extract services
  * Convert existing modules into services.
  * Start with few modules that are easy to extract and then move on to modules that will provide the greatest benefit. These are ones that change frequently or have specific resource requirements.
  * Identify boundaries to break monolith up e.g., a module that communicates with the rest of the app via async messages might be converted into a service.
  * First step is to create coarse grained API between the extracted service and the monolith. Next, convert the coarse API to an IPC mechanism.
  * Over time, the monolith shrinks and you have an increased number of microservices.
