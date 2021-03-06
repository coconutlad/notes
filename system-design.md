# System Design
Notes on what I learnt from [Gaurav Sen's system design playlist](https://www.youtube.com/playlist?list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX). I think that there are a lot of interesting and new ideas involved in designing a modern system and hope to be able to use those concepts while designing and implementing computer systems myself.  

## Scaling systems
Scaling is the process of modifying your system so it can reliably and efficiently provide access to your program to an increasing number of users who might be accessing the system in parallel. You can achieve this by scaling **horizontally**, which means adding multiple similar machines or by scaling **vertically** which means upgrading the machine you have to make it more powerful.  
* Horizontal: Reliable. Easy to add more nodes as number of users increase. Needs load balancing. Inter-node communication (slow). Data inconsistency is possible.
* Vertical: Single point of failure. Harder to upgrade. No load balancing. Intra-process communication (fast). Data is consistent.
Real world systems use a combination of both. They do vertical first and then start adding multiple of those powerful machines.

## An overview
There's high-level-system-design (HLD) which deals with actual systems and an architectural overview of how they interact (which is what the playlist is about) and then there's low-level-system-design (LLD) which deals with code for HLD and diagrams with fine details.  
Some important concepts, which are solutions to technical problems that come up while scaling a system:
* Horizontal & Vertical scaling.
* Backing up - back up data and keep reserve machines ready in case of failures.
* Microservices architecture - Different subsets of tasks are created and they are dealt with in isolation.
* Distributed architecture - Multiple similar systems, to handle high loads.
* Load balancing - Intelligently pass requests to a distributed system based on its info about them.
* Decoupling - Separation of responsibilities amongst unrelated entities in a system.
* Logging - The logs can be analysed to improve the efficiency of the system or detect problems.
* Extensible - The software should be easy to modify in case the requirements change.

## Load balancing
This seemed to be a two part video along with the next topic. Load balancing wrt scaling means distributing incoming requests to servers so that none of them is overwhelmed by requests when another one is relatively free.  
Requests here are represented by request IDs which can be any random integer. They must be mapped to the server space with N nodes 0..N-1. This is done with a hash function (and the mod operator). The hash function's output space is 0..M-1. The problem with `h(r_id) % N` is that when you change N, the request to server mapping is shuffled around a lot. Why is that an issue?
* Given N systems receiving requests, each server will keep some cached information about the requests it receives.
* When another system is added, the requests that each server gets could be vastly different from the ones it used to receive. Which means the cached information needs to be refreshed.  
To make sure that we use as much of the previously cached information as possible our goal is to create a hash function that does not vary its outputs a lot when the number of servers changes. This is discussed in consistent hashing.

## Consistent hashing
The problem of distributing load evenly can be solved with the hash function but modifications are required to make sure it operates smoothly when the number of servers is changed. Firstly, the way we visualise allocation of requests to servers is changed.  
We operate on the hash output space which is 0..M-1. This can be viewed as a circle of slots, like this:
![Consistent hashing circle](./images/consistent_hashing_circle.png)

* First, get the locations of the servers by passing their IDs through the hash function `h(server_id)`.
* Get the location of a request by passing its id through a hash function: `h(r_id)`.
* For each request, find the closest server in the clockwise direction.

This model still has issue of skewing when the number of servers change. Especially when the number of servers is small compared to M. To fix that we **get K locations for each server. Each location is got by `h(server_id + 'i')` where i goes from 0 to K-1.** This creates a more uniform distribution of servers even when there are less servers. A good value for K would be log(M).

## Message queues
A message queue is an asynchronous communication model in a system. It allows a system to receive request from users (and sending them a confirmation) while also processing other requests. Once complete the task is complete the system can send the response with the result to that user.  
The requests received are stored in a ordered list (a database can be used for persistence). Multiple servers are available to process them. A load balancer can distribute the requests among them. It can also take care of the case where a server goes down and its requests have to be passed on to other nodes. For efficiency the database does not store the allocation of requests to servers, it just keeps track of the stage of processing the request is in.  
The load balancer also keeps track of which servers are available via a heart beat system. These two along with the database make up the message queue.

## Microservices and Monoliths
A monolithic architecture is one where all the back-end computation happens on a single machine (which may be distributed and load balancing applied to it). They typically handle a variety of tasks and have a common database instance. A microservice has different machines for different types of back-end services. Those machines can have independent database instances.

Monolithic architectures are good when:
* The system complexity is low or development team is small (since they need to know how the whole system works)
* Communication costs may be too high and it is required that data transfer happens within a program itself.
* A lot of testing and setup code is common among the services and code duplication is not desirable.  
It is often simpler to construct a monolithic system than architect a correct microservice system. StackOverflow uses a monolithic architecture!

Microservices are good when:
* The system needs to be reliable. Since machines usually run a single service a fault in that service will not crash other machines.
* The system is complicated and is best imagined as a collection of communicating services. Helps if the business logic supports this separation of concerns.
* The development team is large and maybe changing. A new person only needs to be familiar with the services they will be using / interacting with.
* The system has to be scaled in a complicated manner. Each service can be scaled individually. They can use different tech stacks.

## Database sharding
The problem? You have one database with a large amount of data and lots of requests. It can be slow, crash, and may not be able to scale. This section is about horizontal partitioning, aka sharding (rows of data are spread over multiple databases unlike vertical partitioning where different columns, i.e. attributes, of all the rows are spread over multiple databases). [This blog post on sharding](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6) says that the criteria in choosing a sharding technique depends on read patterns, data-redistribution techniques and where the data needs to be stored to process requests optimally.  
The data is distributed wrt the partition key. A shard could hold data from a set of partition keys. Data, and therefore requests to that data, can be split among shards via **algorithmic** or **dynamic** sharding. Algorithmic sharding uses a function `f(partition_key) -> shard_id` to decide where to place data. Data-redistribution can be tough, which can be mitigated by using a good mapping function like consistent hashing. Dynamic sharding uses a machine to track where different partitions are stored, e.g. A name node in HDFS.  
For data reliability, each shard can have replication enabled.

## Netflix video distribution
Some things that Netflix does to get a video on to your device:
* They have a large, high quality source video file. They encode it into different formats (each which has differing quality) and for each format they encode it into multiple resolutions. These multiple transformation from the original video to multiple output videos are done in parallel. Thus they have many different video files to send.
* They don't want to send an entire video file to the user. That's too slow and it may be wasteful. First, Netflix splits a video into scenes. A scene is a continuous block of events in the content in the video (e.g. a fighting scene) and it is delivered at a time to the user. Then the decide if the user is likely to watch scene after scene (in which case they'd send the next scene before the current one is over) or jump around to different scenes (in which case they just deliver the scene on demand with a little buffering).
* To reduce transfer times and network traffic over the internet, Netflix has its own CDN called Open Connect. This involves ISPs including Netflix machines, which store data likely to be used in that region, in their servers. They smartly place content there, so efficiently that over 90% of requests for video content are served by Open Connect.