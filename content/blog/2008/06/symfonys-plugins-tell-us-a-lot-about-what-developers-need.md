---
title: "Symfony's plugins tell us a lot about what developers need"
date: "2008-06-21"
tags: [frameworks,programming,webdev]
---

If you want to use the power of the Rails framework without having to learn Ruby, then Symfony is the right framework for you. After spending more than 10 months playing around with Rails, I can say that Symfony is a great alternative to Rails for programmers who already know PHP.

### What is a plugin?

A plugin is a packaged extension for a Symfony project. Plugins can contain classes, helpers, configuration, tasks, modules, schemas and model extensions of their own. The symfony autoloading mechanism takes plugins into account, and the features offered by a plugin are available in the project as if part of the framework. The PEAR packaged plugins have the advantage of managing dependencies, being easier to upgrade and automatically discovered.

### Installing a Plugin

Since I last used Symfony, there have been great improvements, and the plugin system is one of them.

Stefan Koopmanschap [wrote](http://www.symfony-project.org/blog/2008/06/20/how-to-work-with-plugins-in-symfony-1-1):

> For symfony 1.1, the complete plugin system has been rewritten from scratch. This has been done to allow some serious improvements in the way plugins work, and to make it even simpler to work with plugins in your symfony project.
> 
> Installing a plugin has become much easier with symfony 1.1. The whole plugin system is now based on a full implementation of PEAR channels, which enables us to use all the advantages of this, such as the easy management (and even installation) of dependencies, or even the installation of plugins from different PEAR channels than the default symfony channel. You could even set up your own plugin channel to easily install the plugins you've written yourself!

[Learn how to work with plugins in symfony 1.1](http://www.symfony-project.org/blog/2008/06/20/how-to-work-with-plugins-in-symfony-1-1)

### Popular Plugins

A quick look at the analytics of the Symfony website tells us a lot about what people need in addition to the Symfony core. Here is a list of the most popular plugins:

1. [sfSimpleCMSPlugin](http://trac.symfony-project.com/trac/wiki/sfSimpleCMSPlugin)
2. [sfGuardPlugin](http://trac.symfony-project.com/trac/wiki/sfGuardPlugin)
3. [sfDoctrinePlugin](http://trac.symfony-project.com/trac/wiki/sfDoctrine)
4. [sfMediaLibraryPlugin](http://trac.symfony-project.com/trac/wiki/sfMediaLibraryPlugin)
5. [sfSimpleForumPlugin](http://trac.symfony-project.com/trac/wiki/sfSimpleForumPlugin)
6. [sfFeed2Plugin](http://trac.symfony-project.com/trac/wiki/sfFeed2Plugin)
7. [sfThumbnailPlugin](http://trac.symfony-project.com/trac/wiki/sfThumbnailPlugin)
8. [sfUJSPlugin](http://trac.symfony-project.com/trac/wiki/sfUJSPlugin)
9. [sfControlPanelPlugin](http://trac.symfony-project.com/trac/wiki/sfControlPanelPlugin)
10. [sfMogileFSPlugin](http://trac.symfony-project.com/trac/wiki/sfMogileFSPlugin)

If you want to learn more about plugins, how they extend the framework and how you can package the features that you use across several projects into a plugin, read the [plugin chapter](http://www.symfony-project.com/book/1_0/17-Extending-Symfony) of the Symfony book.

The plugin system ensures a great flexibility for distribution, upgrade and clean separation with the rest of the framework.

Once again, I can hear the Symfony play, and it's wonderful.

### Read More

- [Top 20 symfony plugins](http://www.symfony-project.org/blog//2007/08/14/top-20-symfony-plugins)
- [My first symfony project](http://www.symfony-project.org/tutorial/1_0/my-first-project)
- [The Definitive Guide to symfony](http://www.symfony-project.org/book/)
- [The Askeet Tutorial](http://www.symfony-project.org/askeet/1_0/en/)
