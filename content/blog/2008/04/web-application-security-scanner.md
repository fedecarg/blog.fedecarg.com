---
title: "Web Application Security Scanner"
date: "2008-04-24"
tags: [security,tools,web-apps,webdev]
---

Web security is possibly today's most overlooked aspect of securing the enterprise and should be a priority in any organization.

**Recent research shows that 75% of internet attacks are done at web application level.**

Web application security scanners ensure website security by automatically checking for SQL injection, Cross site scripting and other vulnerabilities.

There are a few good security scanners that you can use to test the security of your site, and [Scavenger](https://trac.anl.gov/scavenger/wiki/WikiStart) is one of them.

**Tell me more about Scavenger...**

Scavenger is an open source real-time vulnerability management tool. It helps you respond to vulnerability findings, track vulnerability findings, review accepted or false-positive answered vulnerabilities, and not 'nag' you with old vulnerabilities.

Scavenger parses the results from a Nessus scan and stores them in a MySQL database. From that point, a user can login to a web interface and answer a vulnerability as 'addressed', 'accept', or 'false-positive'. If an administrator answers accept or false-positive, Scavenger will not insert a new vulnerability again. However, if a user marks a vulnerability as 'addressed' and it comes up again in a scan, it will insert a new vulnerability into the database.

[Visit the Website](https://trac.anl.gov/scavenger/wiki/WikiStart)
