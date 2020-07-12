---
title: "Collective Wisdom from the Experts"
date: "2011-06-06"
tags: [software-architecture]
---

I've finally had a chance to read a book I bought a while ago called "[97 Things Every Software Architect Should Know - Collective Wisdom from the Experts](http://www.amazon.co.uk/Things-Every-Software-Architect-Should/dp/059652269X)". Not the shortest title for a book, but very descriptive. I bought this book at the OSCON Conference in Portland last year. It's an interesting book and I'm sure anyone involved in software development would benefit from reading it.

More than 40 architects, including [Neal Ford](http://www.nealford.com/) and [Michael Nygard](http://www.michaelnygard.com/), offer advice for communicating with stakeholders, eliminating complexity, empowering developers, and many more practical lessons they've learned from years of experience. The book offers valuable information on key development issues that go way beyond technology. Most of the advice given is from personal experience and is good for any project leader involved with software development no matter their job title. However, you have to keep in mind that this is a compilation book, so don't expect in-depth information or theoritical knowledge about architecture design and software engineering.

Here are some extracts from the book:

**Simplify essential complexity; diminish accidental complexity** - By Neal Ford

Frameworks that solve specific problems are useful. Over-engineered frameworks add more complexity than they relieve. It's the duty of the architect to solve the problems inherent in essential complexity without introducing accidental complexity.

**Chances are your biggest problem isn't technical** - By Mark Ramm

Most projects are built by people, and those people are the foundation for success and failure. So, it pays to think about what it takes to help make those people successful.

**Communication is King** - By Mark Richards

Every software architect should know how to communicate the goals and objectives of a software project. The key to effective communication is clarity and leadership.

Keeping developers in the dark about the big picture or why decisions were made is a clear recipe for disaster. Having the developer on your side creates a collaborative environment whereby decisions you make as an architect are validated. In turn, you get buy-in from developers by keeping them involved in the architecture process

**Architecting is about balancing** - By Randy Stafford

When we think of architecting software, we tend to think first of classical technical activities, like modularizing systems, defining interfaces, allocating responsibility, applying patterns, and optimizing performance.  Architects also need to consider security, usability, supportability, release management, and deployment options, among others things.  But these technical and procedural issues must be balanced with the needs of stakeholders and their interests.

Software architecting is about more than just the classical technical activities; it is about balancing technical requirements with the business requirements of stakeholders in the project.

**Skyscrapers aren't scalable** - By Michael Nygard

We cannot easily add lanes to roads, but we've learned how to easily add features to software. This isn't a defect of our software processes, but a virtue of the medium in which we work. It's OK to release an application that only does a few things, as long as users value those things enough to pay for them.

**Quantify** - Keith Braithwaite

The next time someone tells you that a system needs to be "scalable" ask them where new users are going to come from and why. Ask how many and by when? Reject "Lots" and "soon" as answers. Uncertain quantitative criteria must be given as a range: the least, the nominal, and the most. If this range cannot be given, then the required behavior is not understood.

Some simple questions to ask: How many? In what period? How often? How soon? Increasing or decreasing? At what rate? If these questions cannot be answered then the need is not understood. The answers should be in the business case for the system and if they are not, then some hard thinking needs to be done.

**Architects must be hands on** - By John Davies

A good architect should lead by example, he/she should be able to fulfill any of the positions within his team from wiring the network, and configuring the build process to writing the unit tests and running benchmarks. It is perfectly acceptable for team members to have more in-depth knowledge in their specific areas but it's difficult to imagine how team members can have confidence in their architect if the architect doesn't understand the technology.

**Use uncertainty as a driver** - By Kevlin Henney

Confronted with two options, most people think that the most important thing to do is to make a choice between them. In design (software or otherwise), it is not. The presence of two options is an indicator that you need to consider uncertainty in the design. Use the uncertainty as a driver to determine where you can defer commitment to details and where you can partition and abstract to reduce the significance of design decisions.

You can purchase "[97 Things Every Software Architect Should Know](http://www.amazon.co.uk/Things-Every-Software-Architect-Should/dp/059652269X)" from Amazon.
