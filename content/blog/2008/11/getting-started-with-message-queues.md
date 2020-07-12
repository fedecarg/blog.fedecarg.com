---
title: "Getting Started With Message Queues"
date: "2008-11-03"
tags: [programming,software-architecture,webdev]
---

When you're building an infrastructure that is distributed all over the internet, you'll come to a point where you can't rely on synchronous remote calls that, for example, synchronize data on 2 servers:

1. You don't have any failover system that resends messages if something went wrong (network outages, software failures).
2. Messages are processed over time and you have no control if something goes overloaded by too many requests.

Even if you don't have to send messages all over the Internet there are enough points of failures where something can go wrong. You want a reliable and durable system that fails gracefully and ensure.

## Solutions

### Dropr

Dropr is a distributed message queue framework written in PHP. The main goals are:

- Reliable and durable (failsafe)-messaging over networks.
- Decentralized architecture without a single (point of failure) server instance.
- Easy to setup and use.
- Modularity for queue storage and message transports (currently filesystem storage and curl-upload are implemented).

[More info](http://blog.northclick.de/archives/34)

### Beanstalkd

Beanstalkd is a fast, distributed, in-memory workqueue service. Its interface is generic, but was originally designed for reducing the latency of page views in high-volume web applications by running most time-consuming tasks asynchronously.

It was developed to improve the response time for the Causes on Facebook application (with over 9.5 million users). Beanstalkd drastically decreased the average response time for the most common pages to a tiny fraction of the original, significantly improving the user experience.

[More info](http://xph.us/software/beanstalkd/)

### Zend Platform Job Queues

Job Queues is an approach to streamline offline processing of PHP scripts. Job Queue Server provides the ability to reroute and delay the execution of PHP scripts that are not essential during user interaction with the Web Server. Job Queues improve the response time during interactive web sessions and utilizes unused resources.

[More info](http://www.zend.com/en/products/platform/product-comparison/job-queues)

### Memcached as simple message queue

In [this post](http://broddlit.wordpress.com/2008/04/09/memcached-as-simple-message-queue/), Olly explains how to use memcached as a simple message queue:

> Some months ago at work we were in the need of a message queue, a very simple one, basically just a message buffer. The idea is simple, the webservers send there messages to the queue, the queue always accepts all messages and waits until the ETL processes request messages for further processing. As the webservers are time critical and the ETL processes aren’t you need something in between.

[More info](http://broddlit.wordpress.com/2008/04/09/memcached-as-simple-message-queue/)

## Links

- [Flickr creates its own PHP messaging server](http://kewnode.wordpress.com/2008/10/03/flickr-creates-its-own-php-messaging-server-to-handle-11-million-tasks-a-day/)
- [Message Queue Project: First working version](http://blog.northclick.de/archives/34)
- [Memcached as simple message queue](http://broddlit.wordpress.com/2008/04/09/memcached-as-simple-message-queue/)
- [Zend Platform Job Queues](http://www.zend.com/en/products/platform/product-comparison/job-queues)
