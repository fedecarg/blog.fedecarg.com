---
title: "Dependency Injection and the Zend Framework"
date: "2008-03-13"
tags: [design-patterns,programming,webdev]
---

A couple of months ago I wrote a [Dependency Injection component](http://framework.zend.com/wiki/display/ZFPROP/Zend_Di+-+Dependency+Injection+Container) for the Zend Framework. It's a simple solution to a complex problem: removing hidden dependencies and injecting mocked objects. The component has evolved quite a bit since I first created it, and it's now part of a bigger system, where objects are persistent, can have different states and are part of an interconnected network of objects.

Pádraic Brady, one of the Zend Framework developers, wrote a nice article about Zend\_Di:

"Dependency Injection is both the ultimate bane and blessing in PHP programming. If you're an experienced object oriented programmer, chances are you already know what the term means, and why it's an all-consuming obsession. If you don't, then here's an overview."

[Rad more...](http://www.developertutorials.com/tutorials/php/the-zend-framework-dependency-injection-and-zend-di-8-02-07/page1.html)
