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


