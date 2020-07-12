---
title: "How to Build a Web Hosting Infrastructure on EC2"
date: "2008-09-29"
tags: [programming,software-architecture,web-services,webdev]
---

_Mike Brittain wrote:_

In the months prior to leaving Heavy, I led an exciting project to build a hosting platform for our online products on top of Amazon’s [Elastic Compute Cloud](http://www.amazon.com/EC2-AWS-Service-Pricing/b/ref=sc_fe_l_2?ie=UTF8&node=201590011&no=3435361&me=A36L942TSJ2AJA) (EC2).  We eventually launched our newest product at Heavy using EC2 as the primary hosting platform.

We set out to build a fairly standard LAMP hosting infrastructure where we could easily and quickly add additional capacity.  In fact, we can add new servers to our production pool in under 20 minutes, from the time we call the “run instance” API at EC2, to the time when public traffic begins hitting the new server.  This includes machine startup time, adding custom server config files and cron jobs, rolling out application code, running smoke tests, and adding the machine to public DNS.

What follows is a general outline of how we do this.

[Continue reading](http://www.mikebrittain.com/blog/2008/07/19/web-hosting-on-ec2/)
