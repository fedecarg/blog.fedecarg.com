---
title: "Apache Log Analyzer 2 Feed"
date: "2009-02-21"
tags: [frameworks,open-source,programming]
---

I found this project thanks to Raphael's post [Turning a Zend\_Log log file into an RSS feed](http://raphaelstolt.blogspot.com/2007/06/turning-zendlog-log-file-into-rss-feed.html).

Developed by Simone Carletti, [ApacheLogAnalyzer2Feed](http://code.simonecarletti.com/wiki/apachelog2feed) is a really powerful open source PHP 5 class to parse and analyse Apache Web Server log files. Results are converted into a feed to let users subscribe them with a feed reader. You can define custom filters based on logs data — for instance User-Agent, IP, requested page, etc— and combine them to select just a limited resultset. The class can easily be extended with additional filters and custom feed handlers.

On a side note, Simone's [Wiki](http://code.simonecarletti.com/wiki/apachelog2feed) is powered by [Redmine](http://www.redmine.org/), an awesome Ruby on Rails Wiki and project management system that's worth checking out.
