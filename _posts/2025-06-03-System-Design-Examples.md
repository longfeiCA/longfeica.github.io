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

# Bitly

## Problem Statement

Design a URL shortener service like bit.ly

## Thinking Process

1. Gather Requirements
2. API Design/Database Design
3. High-Level Design
4. Deep Dives (Scale and Edge cases)

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



## Credits
[Source](https://www.youtube.com/watch?v=qSJAvd5Mgio)

# Google Docs

## Problem Statement

Design a collaborative editing system like Google Docs. 

## Walkthrough

1. Gather Requirements
2. API Design/Database Design
3. High-Level Design
4. Deep Dives

## Credits
[Source](https://www.youtube.com/watch?v=9JKBlkwg0yM)