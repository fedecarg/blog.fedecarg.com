---
title: "Getters and setters create unnecessary coupling"
date: "2008-06-16"
tags: [design-patterns,programming,webdev]
---

Every getter and setter in your code represents a failure to encapsulate and creates unnecessary coupling. A profusion of getters and setters (also referred to as accessors, accessor methods, and properties) is a sign of a poorly designed set of classes.

**"Getters and setters should be avoided because they break the encapsulation OOP offers", says Greg Jorgensen.**

A long time ago programmers discovered that reducing the scope (visibility) of data as much as possible led to more reliable and maintainable code. Before programming languages supported encapsulation or objects, programmers who cared to write better code followed best practices and idioms (what would be called patterns today) that encouraged limiting scope and data hiding. Back in the old days these ideas were discussed in terms of module strength and coupling. To learn the concepts without the distraction of OOP and "design patterns" terminology see Glenford Myers' books "Reliable Software Through Composite Design" and "Composite/Structured Design".

[Read the rest of the article](http://typicalprogrammer.com/?p=23)
