---
title: "How Web Caches Work"
date: "2008-03-11"
tags: [programming,web,webdev]
---

What's a Web Cache? Why do people use them?

A Web cache sits between one or more Web servers (also known as origin servers) and a client or many clients, and watches requests come by, saving copies of the responses, like HTML pages, images and files (collectively known as representations), for itself. Then, if there is another request for the same URL, it can use the response that it has, instead of asking the origin server for it again.

There are two main reasons that Web caches are used:

- **To reduce latency:** Because the request is satisfied from the cache (which is closer to the client) instead of the origin server, it takes less time for it to get the representation and display it. This makes the Web seem more responsive.
- **To reduce network traffic:** Because representations are reused, it reduces the amount of bandwidth used by a client. This saves money if the client is paying for traffic, and keeps their bandwidth requirements lower and more manageable.

[Read more...](http://www.mnot.net/cache_docs/#DEFINITION)
