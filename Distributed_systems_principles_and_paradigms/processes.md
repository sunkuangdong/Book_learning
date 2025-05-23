---
title: "Processes"
date: 2025-04-14
---

# Processes

## INTRODUCTION

In this chapter, we take a closer look at typical organizations of both clients and servers. We also pay attention to general design issues for servers. An important issue, especially in wide-area distributed systems, is moving processes between different machines. What is actually meant by code migration and what its implications are is also discussed in this chapter.

## 3.1 THREADS

Although processes form a building block in distributed systems, practice indicates that the granularity of processes as provided by the operating systems on which distributed systems are built is not sufficient. Instead, it turns out that hav- ing a finer granularity in the form of multiple threads of control per process makes it much easier to build distributed applications and to attain better performance.

### 3.1.1 Introduction to Threads

To keep track of these virtual processors, the operat- ing system has a process table, containing entries to store CPU register values, memory maps, open files, accounting information. The fact that multiple processes may be concurrently sharing the same CPU and other hardware resources is made transparent. Usually, the operating system requires hardware support to enforce this separation. 

Apart from saving the CPU context (which consists of register values, program counter, stack pointer, etc.), the operating system will also have to modify registers of the memory management unit (MMU) and invalidate address translation caches such as in the translation lookaside buffer (TLB). In addition, if the operating system supports more processes than it can simultaneously hold in main memory, it may have to swap processes between main memory and disk before the actual switch can take place.

#### Thread Usage in Nondistributed Systems

#### Thread Implementation
Threads are often provided in the form of a thread package.

Two approaches to thread implementation are:
- User-level threads
- Kernel-level threads

The first approach is to construct a thread library that is executed entirely in user mode. First, it is cheap to create and destroy threads. Because all thread administration is kept in the user's address space, the price of creating a thread is primarily determined by the cost for allocating memory to set up a thread stack. Analogously, destroying a thread mainly involves freeing memory for the stack, which is no longer used. A second advantage of user-level threads is that switching thread context can often be done in just a few instructions. However, a major drawback of user-level threads is that invocation of a blocking system call will immediately block the entire process to which the thread belongs, and thus also all the other threads in that process.

The second approach is to have the kernel be aware of threads and schedule them.

Because the several problems of user-level threads and kernel-level threads, we will pay attention to the LWPs to reslove the problems.

Advantages of LWPs:
* First, creating, destroying, and synchronizing threads is relatively cheap and involves no kernel intervention at all.
* Second, provided that a process has enough LWPs, a blocking system call will not suspend the entire process.
* Third, there is no need for an application to know about the LWPs.
* Fourth, LWPs can be easily used in multiprocessing environ- ments, by executing different LWPs on different CPUs.

Disadvantages of LWPs:
* The only drawback of lightweight processes in combination with user-level threads is that we still need to create and destroy LWPs, which is just as expensive as with kernel-level threads. 

### 3.1.2 Threads in Distributed Systems

An important property of threads is that they can provide a convenient means of allowing blocking system calls without blocking the entire process in which the thread is running.

#### Multithreaded Clients

To establish a high degree of distribution transparency, distributed systems that operate in wide-area networks may need to conceal long interprocess message propagation times. The round-trip delay in a wide-area network can easily be in the order of hundreds of milliseconds or sometimes even seconds.

#### Multithreaded Servers

To understand the benefits of threads for writing server code, consider the organization of a file server that occasionally has to block waiting for the disk. The file server normally waits for an incoming request for a file operation, subse- quently carries out the request, and then sends back the reply.

## 3.2 VIRTUALIZATION

On a single-processor computer, this simultaneous execution is, of course, an illusion. As there is only a single CPU, only an instruction from a single thread or process will be executed at a time. By rapidly switching between threads and processes, the illusion of parallelism is created.

This separation between having a single CPU and being able to pretend there are more can be extended to other resources as well, leading to what is known as resource virtualization.

### 3.2.1 The Role of Virtualization in Distributed Systems

Why virtualization is important for distributed systems?
* One of the most important reasons for introducing virtualization, was to allow legacy software to run on expensive mainframe hardware. 

### 3.2.2 Architectures of Virtual Machines

It is important to realize that computer systems generally offer four different types of interfaces, at four different levels:

1. An interface between the hardware and software, consisting of ma- chine instructions that can be invoked by any program.
2. An interface between the hardware and software, consisting of ma- chine instructions that can be invoked only by privileged programs, such as an operating system.
3. An interface consisting of system calls as offered by an operating system.
4. An interface consisting of library calls, generally forming what is known as an application programming interface (API). In many cases, the aforementioned system calls are hidden by an API.

![image](../assets/image15.png)

Virtualization can take place in two different ways.
* First, we can build a run-time system that essentially provides an abstract instruction set that is to be used for executing applications. 
* Virtual Machine Monitor (VMM)

## 3.3 CLIENTS

### 3.3.1 Networked User Interfaces

A major task of client machines is to provide the means for users to interact with remote servers.

* First, for each remote service the client machine will have a separate counterpart that can contact the service over the network.
* A second solution is to provide direct access to remote services by only offer- ing a convenient user interface. 

![image](../assets/image16.png)

### 3.3.2 Client-Server Programming Paradigms

### 3.3.3 Client-Server Programming in Distributed Systems

