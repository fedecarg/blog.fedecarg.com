---
title: "Memcached consistent hashing mechanism"
date: "2008-12-24"
tags: [programming,software-architecture,webdev]
---

If you are using the Memcache functions through a [PECL extension](http://pecl.php.net/package/memcache), you can set global runtime configuration options by specifying the values within your php.ini file. One of them is **memcache.hash\_strategy**. This option sets the hashing mechanism used to select and specifies which hash strategy to use: Standard (default) or Consistent.

It's recommended that you set to Consistent to allow servers to be added or removed from the pool without causing the keys to be remapped to other servers. When set to standard, an older strategy is used that potentially uses different servers for storage.

With PHP, the connections to the memcached instances are kept open as long as the PHP and associated Apache instance remain running. When adding a removing servers from the list in a running instance, the connections will be shared, but the script will only select among the instances explicitly configured within the script.

So, to ensure that changes to the server list within a script do not cause problems, make sure to use the consistent hashing mechanism.
