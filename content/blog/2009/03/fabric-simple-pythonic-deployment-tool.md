---
title: "Fabric: Simple Pythonic Deployment Tool"
date: "2009-03-05"
tags: [linux,python,tools]
---

Here's the thing: you're developing a server deployed application, it could be a web application but it doesn't have to be, and you're probably deploying to more than one server. Even if you just have one server to deploy to, it still get tiresome in the long run to build your project, fire up your favorite SFTP utility, upload your build, log in to the server with SSH, possibly stop the server, deploy the build, and finally start the server again.

What we'd like to do, is to build, upload and deploy our application with a single command line. Fabric is a tool that, at its core, logs into a number of hosts with SSH, and executes a set of commands, and possibly uploads or downloads files.

[Fabric Project Site](http://www.nongnu.org/fab/)
