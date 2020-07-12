---
title: "Flickr Architecture"
date: "2008-06-30"
tags: [software-architecture,webdev]
---

### Flickr Platform

- PHP
- MySQL
- Shards
- Memcached for a caching layer.
- Squid in reverse-proxy for html and images.
- Linux (RedHat)
- Smarty for templating
- Perl
- PEAR for XML and Email parsing
- ImageMagick, for image processing
- Java, for the node service
- Apache
- SystemImager for deployment
- Ganglia for distributed system monitoring
- Subcon stores essential system configuration files in a subversion repository for easy deployment to machines in a cluster.
- Cvsup for distributing and updating collections of files across a network.

### Flickr Architecture

- [High Scalability: Flickr Architecture](http://highscalability.com/flickr-architecture)
