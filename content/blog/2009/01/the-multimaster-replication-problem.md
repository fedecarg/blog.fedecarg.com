---
title: "The Multimaster Replication Problem"
date: "2009-01-02"
tags: [databases,frameworks,webdev]
---

Replication has its problems, specially if you have a multimaster replication system. To make matters worse, none of the PHP frameworks support multimaster replication systems nor handle master failover. Symfony uses Propel and only supports master-slave replication systems. When the master fails, it's true that you have the slaves ready to replace it, but the process of detecting the failure and acting upon it requires human intervention. Zend Framework, on the other hand, doesn't support replication at all.

I strongly believe that a master failover needs to be handled appropriately on the application side. Of course, you can always use an SQL proxy or any other server-side solution, but they are either limited or unreliable.

From [Digg's Blog](http://blog.digg.com/?p=213):

> The Digg database access layer is written in PHP and lives at the level of the application server. Basically, when the application decides it needs to do a read query, it passes off the query with a descriptor to a method that grabs a list of servers for the database pool that can satisfy the query, then picks one at random, submits the query, and returns the results to the calling method.
> 
> If the server picked won’t respond in a very small amount of time, the code moves on to the next server in the list. So if MySQL is down on a database machine in one of the pools, the end-user of Digg doesn’t notice. This code is extremely robust and well-tested. We worry neither that shutting down MySQL on a read slave in the Digg cluster, nor a failure in alerting on a DB slave that dies will cause site degradation.
> 
> Every few months we consider using a SQL proxy to do this database pooling, failover, and balancing, but it’s a tough sell since the code is simple and works extremely well. Furthermore, the load balancing function itself is spread across all our Apaches. Hence there is no “single point of failure” as there would be if we had a single SQL proxy.

If you are building your own solution and need a sandbox to test it, I recommend using [MySQL Sandbox](http://www.oreillynet.com/databases/blog/2006/05/introducing_the_mysql_sandbox.html). Also, you might find this script useful: [MySQL Master-Master Replication Manager](../2008/10/14/mysql-master-master-replication-manager/ "Permanent Link to MySQL Master-Master Replication Manager")
