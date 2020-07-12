---
title: "A Modular Approach to Web Development"
date: "2008-06-28"
tags: [frameworks,programming,software-architecture,webdev]
---

MVC is about loose-coupling, and Modular Programming takes that concept to the extreme. A modular application can dynamically load and unload modules at runtime, completely separate applications in their own right, which interact with the main application and other modules to perform some set of tasks.

In this article you will presented with a different approach to Web development, one that doesn't focus on a design pattern, instead, it focuses on a programming paradigm known as Modular programming.

# Modularity

Modularity is an important design principle, its goal is to design systems so that modules can be optimised independently of other modules, so that failure of one module does not cause other modules to fail, and in general to make it easier to understand, design and manage complex interdependent systems.

### Principles

1. **Decomposability**: it decomposes a software problem into a small number of less complex sub-problems, connected by a simple structure, and independent enough to allow further work to proceed separately on each item.
2. **Composability**: it favours the products of software elements which may then be freely combined with each other or produce new systems, possibly in an environment quite different from the one in which they were initially developed.
3. **Understandability**: it helps produce software which a human reader can understand each module without having to know the others, or, at worst, by having to examine only a few of the others.
4. **Continuity**: if a small change in the problem specification will trigger a change of just one module, or a small number of modules.
5. **Protection**: if it yields architectures in which the effect of an abnormal condition occurring at run time in a module will remain confined to that module, or at worst will only propagate to a few neighbouring modules.

### Advantages

One of the advantages of modularity, where modules are self contained, is that you can replace or add any module without affecting the rest of the system.

Some frameworks allow you to re-organize your directory structure, but because they were not created with a Modular architecture in mind, they fail to provide most of the advantages that this architecture has to offer.

**Example: Traditional MVC implementation**

```
admin/
    controllers/
    models/
    views/
app/
    controllers/
        ContactUsController
        NewsController
    models/
        ContactUsModel
        NewsModel
    views/
        contactus/
            index.html
        news/
```

Using an MVC framework forces you to divide and organise your code according to the framework conventions. Presentation code goes to the view, data manipulation code goes to the model, and the request manipulation logic goes to the controller.

This is all theory, of course, and even though it's not written anywhere that you should place 200 html files inside a folder called "views", some systems encourage you to do it. And because everyone is doing it, you think is the right thing to do, right? Wrong.

### Modular Programming

Modular programming is the breaking down of a website into parts that are of more manageable size and have a well-defined purpose. Modules should fit intuitively together into a system. In any development environment, modular programming is how larger, more complicated websites are constructed. The first step is to break the task into its basic parts, which leads to defining intermediate steps, and ultimately devising a comprehensive and efficient solution. Modules can be developed individually, validated, and then used throughout an organisation, promoting teamwork, efficiency, and innovation. The advantages of modularity are obvious. Code only needs to be written once, which allows quick modifications. It provides a framework that dictates how subsequent programming should be incorporated. Modularity fits an environment where several programmers share work.

# Modules

A module is a self-contained component of the system, which has a well-defined interface to the other components and can have its own MVC implementation. For example:

```
modules/
    home/
        controllers/
        models/
        views/
    contact-us/
        controllers/
            ContactUsController
        models/
        views/
```

A module can also contain configuration data, user defined routes, dependencies information and language translations:

```
modules/
    contact-us/
        controllers/
        i18n/
            en/
        models/
        properties/
            ContactUsConfig
            ContactUsRoutes
            ContactUsDependencies
        views/
```

### Composability

Composability refers to a collections of modules that can be coordinated and assembled to produce new systems. Pushing this concept a little bit further, we could create a ModuleCoordinator component that acts as a connector between modules. This will allow us to assemble and coordinate new modular subsystems.

### Separation of Concerns

In an MVC framework that implements a modular architecture, separation of concerns (SoC) is the process of breaking a module into distinct features that overlap in functionality as little as possible. A concern is any piece of interest in a module. Based on this concept we are going to separate the functionality of a module into two groups, front-end and back-end:

```
modules/
    contact-us/
        controllers/
            backend/
                ContactUsBackendController
            frontend/
                ContactUsFrontendController
        i18n/
            en/
        models/
        properties/
        views/
            backend/
            frontend/
```

The router of the framework will then determine which module, controller, and action of that controller should receive the request. For example:

**Front-end Action Controller**

The first segment of the URL path maps to the module's front-end controller. The second segment is usually called "action" and maps to a method within that controller.

```
URI:        http://www.example.com/contact-us/support/product/iphone
Module:     contact-us
Controller: ContactUsFrontendController
Method:     supportAction()
Params:     array('product', 'iphone')
```

**Back-end Action Controller**

```
URI:        http://cms.example.com/contact-us/view-departments/10
Module:     contact-us
Controller: ContactUsBackendController
Method:     viewDepartmentsAction()
Params:     array('view', 10)
```

# Content Management

Based on this architecture, you can create a Content Management System (CMS) that allows users to install, activate and assign modules to nodes (or pages). For example:

```
Page               Module          URL               Action
-----------------------------------------------------------------
- Home             home            home              edit | delete
- News             news            latest-news       edit | delete
- Products         products        products          edit | delete
  + Sales          products        on-sales          edit | delete
  + Shops          map             where-to-find-us  edit | delete
- Contact Us       contact-us      contact           edit | delete
```

In the example above, we created a vertical module structure, where the module "maps" depends on the module "products" and inherits its properties. They have a parent/child relationship. However, if you want to minimise dependencies between modules, you can create a flat module structure.

Systems like Joomla and other Content Management Systems have adopted similar architectures, this confirms that a Modular Architecture is a good approach to Web development and RAD.

# Conclusion

A system architecture should not only be based on a design pattern, such as MVC, it should also be based on different programming paradigms.

In a typical website, a design is implemented so that it meets a set of requirements at the time of development. Often, after a website is delivered, the user will want added functionality, or different users will require custom functionality based on specific needs. In order to accommodate these situations without a complete re-write, a framework that allows for future additions of modules without breaking the existing code base needs to be implemented.

In theory, putting the models in the models/ directory, the controllers in the controllers/ directory and the views in the views/ directory makes perfect sense, but in practice, it doesn't. Remember, designing a flexible and scalable system architecture is way beyond your directory structure.
