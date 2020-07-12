---
title: "Detect Replay Attacks in your Web Services"
date: "2008-11-18"
tags: [programming,web-services,webdev]
---

Many threats that are common to distributed systems are common to Web services as well. There are a few specific threats associated with the Web services processing model, such as:

- **Message replays**: An attacker may re-play an entire message or a part of a SOAP message.
- **Man in the middle attack**: An attacker may view and modify a SOAP message without the knowledge of either sender or the receiver.
- **Identity spoofing**: An attempt to construct credentials that seems to be valid but not.
- **Denial of Service (DOS) attacks**: An attempt to make a system expend its resources so that valid requests cannot access a service.
- **Message alteration**: An attempt to alter a message compromising its integrity.
- **Confidentiality issues**: Access to confidential information within a message by unauthorized parties.

Dimuthu wrote an [interesting post](http://www.dimuthu.org/blog/2008/11/17/detect-replay-attacks-in-to-your-php-web-service/) about how to prevent replay attacks using [WSF/PHP](http://wso2.org/projects/wsf/php). He also shows how to detect them using WS-Addressing and WS-Username token headers.
