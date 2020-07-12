---
title: "Website performance monitoring tool"
date: "2017-08-01"
tags: [javascript,node-js,tools,web-apps,web-services]
---

Monitoring systems allow you to monitor changes to your front-end code base over time, catching any regression issues and monitoring the ongoing effects of any performance optimisation changes. Easy to use dashboards are a must when it comes to monitoring the state of your web apps. Companies like Calibre or SpeedCurve offer this as a professional service, but not everyone can afford them.

### Meet SpeedTracker

[SpeedTracker](https://speedtracker.org/) is an open source (MIT license) self-hosted solution to monitor your app's uptime and APIs, developed by Eduardo Bouças. It runs on top of [WebPageTest](https://www.webpagetest.org/) and makes periodic performance tests on your website and shows a visualisation of how the various performance metrics evolve over time.

SpeedTracker provides clean charts and graphs that can help you identify possible problem areas.

![speedtracker01](https://kewnode.files.wordpress.com/2017/08/speedtracker01.png)

Check out the demo here: [https://bbc.github.io/iplayer-web-speedtracker/](https://bbc.github.io/iplayer-web-speedtracker/)

WebPageTest is an incredibly useful resource for any web developer, but the information it provides becomes much more powerful when monitored regularly, rather than at isolated events. Web application monitoring is not just for detecting downtime, it also gives you additional insight into performance trends during peak load times, as well as by time of day, and day of the week.

![speedtracker04](https://kewnode.files.wordpress.com/2017/08/speedtracker04.png)

For me, the best thing about SpeedTracker is that **it runs on your GitHub repository!** Data from WebPageTest is pushed to a GitHub repository. It can be served from GitHub Pages, from a private or public repository, with HTTPS baked in for free.

SpeedTracker also allows you to define performance budgets for any metric you want to monitor and receive alerts when a budget is overrun. **This can be an e-mail or a message on Slack.**

For instructions on how to install this tool, visit the following GitHub repo: [https://github.com/speedtracker/speedtracker](https://github.com/speedtracker/speedtracker)
