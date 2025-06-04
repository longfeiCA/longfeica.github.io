---
layout: post
title: System Design Examples
subtitle: System design
tags: [System Design, REST API, SQL, database, DBMS]
comments: false
mathjax: false
author: Longfei Jiao
last-updated: 2025-06-03
---

# **Bitly (URL Shortener)**

## Problem Statement

Design a URL shortener service like bit.ly

## Thinking Process

1. Gather Requirements
2. API Design/Database Design
3. High-Level Design
4. Deep Dives (Scale and Edge cases)

Scale: Consider storage, latency and throughput. 

## Gather Requirements

### Functional Requirements
1. URL Shortening - pass in a long url and get a unique short url
2. URL Redirection - Short URL -> Long URL
3. Link Analytics - Be able to track the number of times each short url is accessed

### Non-Functional Requirements
Performance: 
1. Minimize redirect latency
2. 100M Daily Active Users
3. 1B reads per day = 10K req/s
4. 1-5B lifetime URLs

## API Design

Shortening:

    POST /api/urls/shorten
    Req: Long URL
    Res: Short URL

Navigation: 

    GET /api/urls/{shortURL}
    Res: Redirect 302/307

    PS: Get request does not have a body
    301 will be cached, but 302/307 will not be cached

### High-Level Design

Micro-service architecture

    Client -> API Gateway -> URL service(s) -> Database

URL Services: 
    URL Redirection Service
    URL Shortening Service

### Deep Dive

#### How many characters do we need in a short URL?

We may use the Base-62 characters (alphanumeric characters, including A-Z, a-z, 0-9), and we put 6 characters in the URL (62^6 = 56B, which is sufficient). 

#### Database Table

Short URL

    short_url: String
    long_url: String
    uid: String
    created_at: Date
    used_count: int

#### Calculating Storage

    say, each record use 1kb of Data: 
    1kb * 50B = 10^3 * 50*10^9 = 5*10^13 bit =  6.25 TB

So storing all the data in a single database is fine. 

#### How to generate unique urls? 

1. Use a Counter

Concurrency Issues - We may have multiple counters responsible for multiple spaces. Each counter is single threaded. 

2. Use a Hashing Function

Collision problem - It is not a big problem because we can add another character to the url if collisions happen often. 


#### Latency & Throughput

1. Single Node DB
10k req/s or even 100k req/s is OK if we scale vertically, since the requests are just simple read operations. We should index the short_url to improve the redirection speed. 

2. If we need to split the database, we may use one leader database and multiple replica databases that handles extra read requests. 

3. Another great idea is to use cache to store frequently used short urls. Memory is much faster so it can handle much more throughput and in the real world, most short urls will be used for only a week or two then they'll never be used again. 

#### How to update the used_count efficiently? 

We need to store the used_count in the database but make a write request is much more expensive than a read request. 

We may use a in-memory cache to store the count temporarily and flush it every 60 seconds as a cron job. 


## Credits
[Source](https://www.youtube.com/watch?v=qSJAvd5Mgio)

# **Google Docs**

## Problem Statement

Design a collaborative editing system like Google Docs. 

## Walkthrough

1. Gather Requirements
2. API Design/Database Design
3. High-Level Design
4. Deep Dives

## Credits
[Source](https://www.youtube.com/watch?v=9JKBlkwg0yM)