---
title: "BBC's New Infrastructure: Java and PHP"
date: "2008-06-02"
tags: [frameworks,java,programming,software-architecture,webdev]
---

Like most organizations the BBC has its own technical ecosystem, the BBC's is pretty much restricted to Perl and static files. The good news is that they are planning to build a new infrastructure for bbc.co.uk and roll out a new Identity platform on it. This was announced on May during the XTech 2008 presentation that took place in Dublin, where BBC's plans to reinvigorate its technical platform were revealed.

### Why is the BBC doing this?

- Gradual evolution of the web.
- Antiquated web platform.
- Inflexible / proprietary architecture.
- Single Sign-On (SSO) was unable to adapt.

### Architectural principles

1. Each project must have a clear customer and deliver a real benefit.
2. Don't Repeat Yourself (DRY). Life is too short to spend your time re-inventing things.
3. Be as simple as possible. Just do what we need to do now.
4. Be as open as possible. Assume that all services can be accessed from outside the BBC, by default.
5. Start simple, then iterate. Build the smallest thing you could possibly need, deploy it, then build applications on top of it. Think building blocks, not monoliths.
6. Don't optimise prematurely. The service might not grow the way we think it will.
7. Build to scale. Think stateless, think content delivery networks, think database resilience.
8. Test often. So you know when you need to optimise. So you can maintain your code. So you can maintain your platform.
9. Evolve. Teams, systems, support structures. The platform. These principles!
10. Let it die. Be prepared to turn your system off, or change it unrecognisably.

### Architecture

- Service Oriented Architecture
- MySQL Repository
- [OpenSSO](https://opensso.dev.java.net/) Identity Management Core
- Java REST Services: [Spring MVC](http://www.springframework.org/)
- PHP presentation layer: [Zend Framework](http://framework.zend.com/)

### Benefits of using the Zend Framework

- Zend Framework's extensive unit tests, required for all contributed code, mean that exact situations can be recreated and problems diagnosed and pinpointed.
- The high-quality implementation of the MVC web application architecture provides a foundation for all Zend Framework applications.
- Internationalization (i18n) and Localization (l10n).
- Session management.
- Ajax support.
- Security.
- Documentation.
- Zend Framework tools (Wiki, Issue Tracker, VCS, Mailing List, etc).
- A vast community of talented developers.

### XTech 2008 in Dublin

Presentation from XTech 2008 in Dublin, about the BBC's plans to reinvigorate its technical platform and to create new user management service.

- [XTech 2008, BBC Tech Refresh and Identity](http://www.slideshare.net/bquinn/xtech-2008-bbc-tech-refresh-and-identity-brendan-quinn-and-ben-smith)

### References

- [@media2008](http://www.vivabit.com/atmedia2008/london/sessions/#forexample)
- [Sitepoint](http://www.sitepoint.com/blogs/2008/05/30/media2008-day-one/)
