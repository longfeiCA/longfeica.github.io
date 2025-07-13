---
layout: post
title: Design Data Intensive Application
subtitle: The big ideas behind reliable, scalable, and maintainable systems. 
tags: [backend, system design]
comments: false
mathjax: false
author: Longfei Jiao
last-updated: 2025-05-28
---

# [Full Notes](https://github.com/longfeiCA/notes-designing-data-intensive-applications/)

## Goal
The goal of this book is to help you navigate the diverse and fast-changing landscape of technologies for processing and storing data. You will understand how tools can be combined to form the foundation of a good application architecture. 

## Design Software that has:

1. **Reliability**: Tolerating hardware & software faults; Human error

2. **Scalability**: Measuring load & performance; Latency

3. **Maintainability**: Operability, simplicity & evolvability


## Scope & Arrangements

### Scope
1. No technical details on how to install or use specific software packages or APIs. 
2. This book doesn’t have space to cover deployment, operations, security, management, and other areas—those are complex and important topics, and we wouldn’t do them justice by making them superficial side notes in this book. They deserve books of their own.

### Arrangements
This book is arranged in three parts:  

1. In Part I, we discuss the fundamental ideas that underpin the design of data-intensive applications. We start in Chapter 1 by discussing what we’re actually trying to achieve: reliability, scalability, and maintainability; how we need to think about them; and how we can achieve them. In Chapter 2 we compare several different data models and query languages, and see how they are appropriate to different situations. In Chapter 3 we talk about storage engines: how databases arrange data on disk so that we can find it again efficiently. Chapter 4 turns to formats for data encoding (serialization) and evolution of schemas over time.

2. In Part II, we move from data stored on one machine to data that is distributed across multiple machines. This is often necessary for scalability, but brings with it a variety of unique challenges. We first discuss replication (Chapter 5), partitioning and sharding (Chapter 6), and transactions (Chapter 7). We then go into more detail on the problems with distributed systems (Chapter 8) and what it means to achieve consistency and consensus in a distributed system (Chapter 9).

3. In Part III, we discuss systems that derive some datasets from other datasets. Derived data often occurs in heterogeneous systems: when there is no one database that can do everything well, applications need to integrate several different databases, caches, indexes, and so on. In Chapter 10 we start with a batch processing approach to derived data, and we build upon it with stream processing in Chapter 11. Finally, in Chapter 12 we put everything together and discuss approaches for building reliable, scalable, and maintainable applications in the future.