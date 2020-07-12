---
title: "Domain-Driven Design and MVC Architectures"
date: "2009-03-11"
tags: [agile-development,design-patterns,frameworks,programming,software-architecture]
---

According to [Eric Evans](http://domaindrivendesign.org/), Domain-driven design (DDD) is not a technology or a methodology. It's a different way of thinking about how to organize your applications and structure your code. This way of thinking complements very well the popular MVC architecture. The domain model provides a structural view of the system. Most of the time, applications don't change, what changes is the domain. MVC, however, doesn't really tell you how your model should be structured. That's why some frameworks don't force you to use a specific model structure, instead, they let your model evolve as your knowledge and expertise grows.

Domain-driven design separates the model layer "M" of MVC into an application, domain and infrastructure layer. The infrastructure layer is used to retrieve and store data. The domain layer is where the business knowledge or expertise is. The application layer is responsible for coordinating the infrastructure and domain layers to make a useful application. Typically, it would use the infrastructure to obtain the data, consult the domain to see what should be done, and then use the infrastructure again to achieve the results. Srini Penchikala explains this in more detail here: "[Domain Driven Design and Development In Practice](http://www.infoq.com/articles/ddd-in-practice)".

Object-oriented programming is the most important element in the domain implementation. Domain objects are designed using classes and interfaces, and take advantage of OOP concepts like inheritance, encapsulation, and polymorphism. Most of the domain elements are true objects with both State (attributes) and Behaviour (methods or operations that act on the state). Entities and Value Objects in DDD are classic examples of OOP concepts since they have both state and behaviour.

### Terminology used by Domain-driven design

- **Entity**: An object which has a distinct identity. For example, a User entity has a distinct identity with an ID.
- **Value Object**: An object which has no distinct identity, like numbers and dates. For example, if two Users have the same date of birth, you can have multiple copies of an object that represents the date 16 Jan 1982.
- **Factory**: Used to create Entities. For example, you can use a Factory to create a Profile entity from a User entity.
- **Repository**: Used to store, retrieve and delete domain objects from different storage implementations. For example, you can use a Repository to store the Profile your Factory created.
- **Service**: When an operation does not conceptually belong to any object.

The purpose of this post was to provide a brief introduction to Domain-driven design.

Part 2: [Domain-Driven Design: Data Access Strategies](http://blog.fedecarg.com/2009/03/12/domain-driven-design-and-data-access-strategies/)

### Links

If you're interested in learning more about Domain-driven design or Model-driven design, I recommend the following articles:

- [Domain Driven Design and Development In Practice](http://www.infoq.com/articles/ddd-in-practice)
- [Domain-Driven Design in an Evolving Architecture](http://www.infoq.com/articles/ddd-evolving-architecture)
- [Zend Framework: Model Infrastructure](http://weierophinney.net/matthew/archives/202-Model-Infrastructure.html)
- [Why Models are Misunderstood and Unappreciated](http://blog.astrumfutura.com/archives/373-The-M-in-MVC-Why-Models-are-Misunderstood-and-Unappreciated.html)
