---
title: "Check whether your web server is correctly configured"
date: "2011-12-11"
tags: [deployment,linux,open-source,security,tools]
---

Last year [Zone-H](http://twitter.com/#!/fedecarg/statuses/87465788595900416) reported a record number of 1.5 million websites defacements. 1 million of those websites where running Apache.

When it comes to configuring a web server, some people tend to turn everything on by default. Developers are happy because the functionality that they wanted is available without any extra configuration, and there is a reduction in support calls due to functionality not working out-of-the-box. This has proven to be a major source of problems for security in general. A web server should start off with total restriction and then access rights should be applied appropriately.

You can check whether your web server is correctly configured by using [Nikto](http://www.cirt.net/nikto2), a great open source vulnerability scanners that is able to scan for quite a large number of web server vulnerabilities. From their site:

_"Nikto is an Open Source (GPL) web server scanner which performs comprehensive tests against web servers for multiple items, including over 6400 potentially dangerous files/CGIs, checks for outdated versions of over 1200 servers, and version specific problems on over 270 servers. It also checks for server configuration items such as the presence of multiple index files, HTTP server options, and will attempt to identify installed web servers and software. Scan items and plugins are frequently updated and can be automatically updated."_

I'm going to run a default scan by just supplying the IP of the target:

```
$ cd nikto-2.1.4
$ ./nikto.pl -h 127.0.0.1

- ***** SSL support not available (see docs for SSL install) *****
- Nikto v2.1.4
---------------------------------------------------------------------------
+ Target IP:          127.0.0.1
+ Target Hostname:    localhost.localdomain
+ Target Port:        80
+ Start Time:         2011-12-12 13:06:59
---------------------------------------------------------------------------
+ Server: Apache
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ 6448 items checked: 0 error(s) and 0 item(s) reported on remote host
+ End Time:           2011-12-12 13:08:07 (68 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

By looking at the last section of the Nikto report, I can see that there are no issues that need to be addressed.

Tools like [Nikto](http://www.cirt.net/nikto2) and [Skipfish](http://code.google.com/p/skipfish/) serve as a foundation for professional web application security assessments. Remember, the more tools you use, the better.

### Links

- [http://www.cirt.net/nikto2](http://www.cirt.net/nikto2)
- [http://code.google.com/p/skipfish](http://code.google.com/p/skipfish)
- [http://sectools.org](http://sectools.org)
