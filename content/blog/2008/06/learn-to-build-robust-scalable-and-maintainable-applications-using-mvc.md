---
title: "Learn to Build Robust, Scalable and Maintainable Applications using MVC"
date: "2008-06-25"
tags: [frameworks,programming,software-architecture]
---

MVC is about loose-coupling, and Modular Programming takes that concept to the extreme. A modular application can dynamically load and unload modules at runtime, completely separate applications in their own right, which interact with the main application and other modules to perform some set of tasks

[This document (PDF)](http://puremvc.org/pages/docs/current/PureMVC_Framework_Overview_with_UML.pdf) discusses the classes and interfaces of the PureMVC framework; illustrating their roles, responsibilities and collaborations with simple UML diagrams. The PureMVC framework has a very narrow goal, that is to help you separate your application's coding concerns into three discrete tiers; Model, View and Controller. In this implementation of the classic MVC design meta-pattern, the application tiers are represented by three Singletons (a class where only one instance may be created). A fourth Singleton, the Façade, simplifies development by providing a single interface for communications throughout the application.

### PureMVC Framework

- [Framework Overview (PDF)](http://puremvc.org/pages/docs/current/PureMVC_Framework_Overview_with_UML.pdf)
- [Website](http://puremvc.org/)
