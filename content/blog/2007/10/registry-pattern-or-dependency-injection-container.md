---
title: "Registry Pattern, good or bad?"
date: "2007-10-26"
tags: [design-patterns,programming,webdev]
---

Today I came across an [interesting post](http://www.sitepoint.com/forums/forumdisplay.php?f=147) written by Troels Knak-Nielsen. We all know that patterns are not perfect in all situations and the Registry pattern is no exception. Here is what Troels has to say about this:

**The Registry Pattern** - By Troels Knak-Nielsen

The task, that a registry aims to solve, is to get shared objects (the dependencies) into an object, which needs it (the dependent). There are several ways of solving that task and a registry is one of them. Or rather... It's two. The registry can denote a globally accessible container for shared objects, but it can also denote a local (passed around) container. In lack of better terms, we can call them a global registry vs a local registry. When people talk about a registry, they usually mean the global variant.

If the definition of a registry is a container for shared objects, then the simplest possible implementation is an array. The simplest variant of a global registry is then simply $GLOBALS. In contrast, a passed-around array of instances is an equally primitive local registry. One can use an object to implement a more sophisticated variant of a registry, but it's still fulfilling the same role as the primitive variant.

The problem with a global registry -- whether it's primitive or sophisticated -- is that it's a global symbol. Most programmers will agree that global variables are bad design, and that goes for a global registry as well. Even though this is common knowledge, a lot of people aren't aware, that fancy named patterns, such as Singleton or Registry are types of global variables and therefore have the same fundamental problems as other global variables do. The point of me comparing (a global) registry to $GLOBALS, was to emphasize this matter.

Now, even if you use a local registry (eg. pass it around), there are still problems with it. When passing a registry, containing dependencies, rather than passing the actual dependencies, there are two problems. The first is, that you introduce an additional dependency (Eg. the dependency on the registry). This is a minor issue though; Much worse is it, that it becomes less clear, which actual dependencies, the dependent has. With the passing-style, you can always see in the signature of an objects constructor, which are the dependencies. With a registry-style, you must read through the actual code.

The registry offers one benefit over passing-style; It's much easier to change the dependencies, because you only have to make changes to the dependent. Using passing-style, you'll have to update any classes, which uses the dependent.

The other option is a dependency injection container. This has the benefits of both worlds, without any of the problems. The problem with it is, that it's a rather large construct, so for smaller applications, it just feels like overkill.

All considered, a local registry isn't something I would detest. I prefer passing-style though. If it gets unwieldy, I would rather switch to an injection container, than a registry.
