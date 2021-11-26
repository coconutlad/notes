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
* Extensible - The software should easy to modify in case the requirements change.
