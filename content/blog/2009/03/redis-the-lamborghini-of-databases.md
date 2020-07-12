---
title: "Redis: The Lamborghini of Databases"
date: "2009-03-09"
tags: [databases,linux,open-source,python,webdev]
---

Antonio Cangiano [wrote](http://antoniocangiano.com/2009/03/11/introducing-redis-a-key-value-database/):

[Redis](http://code.google.com/p/redis/) is a key-value database written in C. It can be used like memcached, in front of a traditional database, or on its own thanks to the fact that the in-memory datasets are not volatile but instead persisted on disk. Redis provides you with the ability to define keys that are more than mere strings, as well as being able to handle multiple databases, lists, sets and even basic master-slave replication. It's amazingly fast. On an entry level Linux box, Redis has been benchmarked performing 110,000 SET operations, and 81,000 GETs, per second.

Despite being a very young project, it already has client libraries for several languages: Python and PHP, Erlang, and Ruby. Salvatore Sanfilippo, the creator of Redis, has implemented a Twitter clone known as [Retwis](http://retwis.antirez.com/) to showcase how you can use Redis and PHP to build applications without the need for a database like MySQL or any SQL query.

Salvatore will be publishing a beginner’s article based on the PHP Twitter clone he wrote, soon.

Full article: [Introducing Redis: a fast key-value database](http://antoniocangiano.com/2009/03/11/introducing-redis-a-key-value-database/)
