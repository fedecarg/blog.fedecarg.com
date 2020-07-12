---
title: "Command-line memcached stat reporter"
date: "2009-11-29"
tags: [open-source,tools]
---

[Nicholas Tang](http://nicholasytang.wordpress.com/) wrote a nice little perl script that shows a basic memcached top display for a list of servers. You can specify thresholds, for instance, and it'll change color to red if you exceed the thresholds. You can also choose the refresh/sleep time, and whether to show immediate (per second) stats, or lifetime stats.

To install it you only need to download the script and make it executable:

```
$ curl http://memcache-top.googlecode.com/files/memcache-top-v0.6 > ~/bin/memcache-top
$ chmod +x ~/bin/memcache-top
$ memcache-top --sleep 3 --instances 10.50.11.3,10.50.11.4,10.50.11.5
```

Here's some sample output:

```
memcache-top v0.6       (default port: 11211, color: on, refresh: 3 seconds)

INSTANCE                USAGE   HIT %   CONN    TIME    EVICT/s GETS/s  READ/s  WRITE/s
10.50.11.3:11211        88.9%   69.7%   1661    0.9ms   0.3     47      13.9K   9.8K
10.50.11.4:11211        88.8%   69.9%   2121    0.7ms   1.3     168     17.6K   68.9K
10.50.11.5:11211        88.9%   69.4%   1527    0.7ms   1.7     48      14.4K   13.6K
AVERAGE:                84.7%   72.9%   1704    1.0ms   1.3     69      13.5K   30.3K

TOTAL:          19.9GB/ 23.4GB          20.0K   11.7ms  15.3    826     162.6K  363.6K
(ctrl-c to quit.)
```

Project Home [http://code.google.com/p/memcache-top/](http://code.google.com/p/memcache-top/)
