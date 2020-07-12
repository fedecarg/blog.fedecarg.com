---
title: "MySQL Master-Master Replication Manager"
date: "2008-10-14"
tags: [databases,tools]
---

The [MySQL Master-Master replication](http://code.google.com/p/mysql-master-master/) (often in active-passive mode) is popular pattern used by many companies using MySQL for scale out. Most of the companies would have some internal scripts to handle things as automatic fallback and slave cloning but no Open Source solution was made available.

In 2007, the High Performance Group at MySQL AB was asked to implement such solution for one of their customers and they kindly agreed to let them release the software under GPL2 license.

The MySQL Master-Master Replication Manager (MMM) is a set of flexible scripts to perform monitoring/failover and management of MySQL Master-Master replication configurations (with only one node writable at any time). The toolset also has the ability to read balance standard master/slave configurations with any number of slaves, so you can use it to move virtual IP addresses around a group of servers depending on whether they are behind in replication.

### Links

- [MMM Installation](http://blog.kovyrin.net/mysql-master-master-replication-manager/installation-instructions/)
- [Master-Master Replication Example using MMM](http://blog.kovyrin.net/2007/04/23/master-master-replication-example-using-mmm/)
- [Typical use-cases of mmm-deployment](http://blog.kovyrin.net/mysql-master-master-replication-manager/use-cases/)
- [Project page on Google Code](http://code.google.com/p/mysql-master-master/)
