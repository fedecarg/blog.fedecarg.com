---
title: "A Painless Remote Projects Synchronization Utility"
date: "2008-04-29"
tags: [frameworks,programming,tools]
---

So, there you are, asking yourself - "How the hell am I going to deploy my next application across multiple servers?". First, you write some shell scripts. One that checks out the project from the repository and the other one that runs all the tests. Then, you master the synchronization technique and realise that combined with the power of a Cron job, nothing and no one can stop you. Finally, you become a Jedi.

But, unfortunately, this is not a movie and you are not Luke Skywalker. So, forget about reinventing the wheel and use [Syncman](http://syncman.limb-project.com/) instead.

**What is Syncman?**

Syncman is a [Limb3](http://limb-project.com/) based application developed by Pachanga (LIMB, WACT, SimpleTest). It simplifies projects remote deployment and synchronization by providing both nice Web UI and basic shell interface.

**Features:**

- Nice web UI for non-technical personnel
- Simple file based projects configuration
- Public keys infrastructure for secure passwordless authentication
- Efficient rsync based synchronization(but not limited to rsync)
- Subversion integration
- Pre and Post-syncing hooks support
- Shell based interface

[Visit the site](http://syncman.limb-project.com/)
