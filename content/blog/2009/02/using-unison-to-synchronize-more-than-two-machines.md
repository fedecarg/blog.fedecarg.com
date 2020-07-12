---
title: "Using Unison to synchronize more than two machines"
date: "2009-02-03"
tags: [linux,open-source,tools]
---

Rsync is great, however, it only synchronizes files in one direction. [Unison](http://www.cis.upenn.edu/~bcpierce/unison/), on the other hand, synchronizes both ways. It allows two replicas of a collection of files and directories to be stored on different hosts, modified separately, and then brought up to date by propagating the changes in each replica to the other.

Why you should use Unison instead of Rsync:

- Unison works across platforms, allowing you to synchronize a Windows laptop with a Unix server, for example.
- Unlike simple mirroring or backup utilities, Unison can deal with updates to both replicas of a distributed directory structure. Updates that do not conflict are propagated automatically. Conflicting updates are detected and displayed.
- Unlike a distributed filesystem, Unison is a user-level program: there is no need to modify the kernel or to have superuser privileges on either host.
- Unison works between any pair of machines connected to the internet, communicating over either a direct socket link or tunneling over an encrypted ssh connection. It is careful with network bandwidth, and runs well over slow links such as PPP connections. Transfers of small updates to large files are optimized using a compression protocol similar to rsync.
- Unison is resilient to failure. It is careful to leave the replicas and its own private structures in a sensible state at all times, even in case of abnormal termination or communication failures.

### Links

- [Project Homepage](http://www.cis.upenn.edu/~bcpierce/unison/)
- [Unison User Manual and Reference Guide](http://www.cis.upenn.edu/~bcpierce/unison/download/releases/stable/unison-manual.html)
- [Keep your laptop and PC in Unison](http://www.linux.com/feature/118842)
