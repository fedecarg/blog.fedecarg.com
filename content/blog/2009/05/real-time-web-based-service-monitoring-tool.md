---
title: "Real Time Web-Based Service Monitoring Tool"
date: "2009-05-12"
tags: [open-source,tools,web-apps,webdev]
---

[phpWatch](http://aaron-rosenfeld.com/2008/08/28/phpwatch-release-announcement/) is a general purpose service monitor that is able to send notifications of outages via e-mail or text-message (SMS). The purpose of this system is to allow administrators to easily check the status of many different services running on any number of servers and also allow developers to interface with the query and notification APIs.

### Installation

The basic installation is very simple: chmod config.php to 777 and simply navigate to the install directory in your browser. Fill in the database information and the setup will create the required tables and setup the configuration file as needed. The only required task beyond the automated install is to add cron.php as a cron job (Unix/Linux) or scheduled task (Windows).

### SMS Alerts

phpWatch uses pre-existing SMS gateways provided by the cell-carriers themselves. For example, to send a message to a Verizon subscriber with the phone number 123-456-7890, an e-mail can be sent to 1234567890@vtext.com and it will then be forwarded to the individual's phone.

### Links

- [Demo](http://aaron-rosenfeld.com/phpWatch/demo/)
- [Download phpWatch v1.0.8 Beta](http://aaron-rosenfeld.com/2009/04/07/phpwatch-v108-beta-released/)
- [Documentation](http://aaron-rosenfeld.com/phpWatch/demo/docs/)
