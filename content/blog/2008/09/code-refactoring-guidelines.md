---
title: "Code Refactoring Guidelines"
date: "2008-09-14"
tags: [programming,software-architecture,webdev]
---

In software engineering, "refactoring" source code means modifying it without changing its behaviour, and is sometimes informally referred to as "cleaning it up". Refactoring neither fixes bugs nor adds new functionality, though it might precede either activity. Rather it improves the understandability of the code and changes its internal structure and design, and removes dead code, to make it easier to comprehend, more maintainable and amenable to change. Refactoring is usually motivated by the difficulty of adding new functionality to a program or fixing a bug in it.

Code Refactoring Guidelines

1. Big Picture
2. Extreme Abstraction
3. Extreme Separation
4. Extreme Readability
5. Interfaces
6. Error Handling
7. General Issues
8. Security
9. General Objects

# Big Picture

**Don't reinvent the wheel** Look for existing solutions to problems before creating new solutions.

**Think about the big picture** Decisions within a system should be congruent with the big picture.

**Document your assumptions and your decisions** Keep a journal for retrospectives.

**Don't Repeat Yourself (DRY)** Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

**Plan globally, develop locally** Incremental implementations should fit into a global plan.

# Extreme Abstraction

**Splitters can be lumped more easily than lumpers can be split** It is easier to combine two concepts than it is to separate them.

**Clump data so that there is less to think about** Clumping data cuts down on the number of concepts that have to be kept in mind.

**When you're abstract, be abstract all the way** Do not describe data items using primitive data types.

**Strings are more than just a string** Treat String as a primitive data type. Describe attributes with abstract data types, instead of as Strings.

**Never let a constant slip into code** Use a symbolic name for all values.

**To text or not to text** Use text between programs, not within programs.

**If it has collection operations, make it a collection** Collections separate object usage from object storage and hide implementation of aggregate operations.

**Don't change what it is** Create new terms rather than trying to apply new meanings to current terms

# Extreme Separation

**Adapt a prefactoring attitude** Eliminate duplication before it occurs.

**Don't overclassify** Separate concepts into different classes based on behavior, not on data. **Place methods in classes based on what they need** If a method does not require instance data, it should not be a member of the class. Conversely, if all the method requires is the instance data, it should be a member of the class.

**Honour the class maxims** Make loosely coupled cohesive classes.

**Do a little job well and you may be called upon often** Methods and classes that perform specific jobs can be reused more often.

**Separate policy from implementation** Keeping the what separated from the how makes the what more readable and maintainable.

**Separate concerns to make smaller concerns** Split responsibilities among multiple methods and multiple classes to simplify each method and class.

**Test or Production; that is the question** Place all test-only methods in a test interface.

**Build flexibility for testing** Plan for flexibility in your design to allow for ease of testing.

**Decouple with associations** Association classes decouple the two classes being associated. **Split interfaces** Split a single interface into multiple interfaces if multiple clients use different portions of the interface.

**Do a little and pass the buck** Add proxies to interfaces to add functionality.

**Business rules are a business unto themselves** Keep business rules separate from other logic.

# Extreme Readability

**A rose by any other name is not a rose** Create a clearly defined name for each concept in a system.

**Prototypes are worth a thousand words** A picture of an interface, such as a screen, can be more powerful than just a description.

**Communicate with your code** Your code should communicate its purpose and intent.

**Explicitness beats implicitness** Being explicit reduces misinterpretation.

**Declaration over execution** Declarative-style programming can provide flexibility without code changes.

**Use the same layout to get the same layout** Use templates or scripts for classes and methods to create consistent logic.

**The easiest code to debug is that which is not written** Never write functionality that already exists in usable form.

**Use the client's language** Use the client's language in your code to make it easier to compare the logic in the code to the logic of the client.

# Interfaces

**Create interface contracts** Design with well-defined interfaces and enforce the contracts for those interfaces.

**Validate, validate, validate** At each interface, validate that the input matches the contract.

**Test the interface, not the implementation** Use the contract of the interface to develop the functional tests, not the implementation behind it.

**Adopt and adapt** Create the interface you desire and adapt the implementation to it. **Don't let the cold air In** With interfaces exposed to the outside world, ensure that input validation and logging occurs.

# Error Handling

**Decide on a strategy to deal with deviations and errors** Determine for your system what are deviations and what are errors, and how to deal with both.

**Report meaningful user messages** Error messages should be reported in the context of what the user can do about the error, instead of in terms of what the underlying error is.

**Never be silent** If a method encounters an error, it should report it, not remain silent.

**Consider failure an expectation, not an exception** Plan how operations should respond to failures.

# General Issues

**Don't speed until you know where you are going** Make the system right, before you make it fast.

**The spreadsheet conundrum** Recognize when you are making the row/column decision.

**Consistency is simplicity** A consistent approach to style and solutions can make code easier to maintain.

**If it can't be tested, don't require it** Every functionality requirement, whether formally or informally stated, should have a test created for it. If you cannot test a requirement, there is no way to determine whether you have met it.

**Plan for testing** Developing test strategies in advance can lead to a better design.

**Figure out how to migrate before you migrate** Considering the migration path might help you discover additional considerations in other areas of the design.

**Know who it Is** Determine uniqueness criteria for objects that should be unique.

**Perform a retrospective after each release** Examining your design and how you created it can help in the next release.

**Nothing is perfect** There is usually a better solution, but you can stop with good enough.

**See what condition your condition is in** Use state-based analysis to examine object behavior.

**Get something working** Create something basic before adding refinements.

**Plan your logging strategy** Determine where and how you are going to log.

**More is sometimes less** Use a prewritten module with more features than you currently need and adapt it to your current needs.

**Be ready to import and export** Data should be available for use outside the system via a well-defined data interface. **Avoid premature generalization** Solve the specific problem before making the solution general.

# Security

**If you forget security, you're not secure** Security should not be an afterthought. Consider it during all phases of development.

**Consider privacy** Systems need to be designed with privacy in mind.

# General Objects

**Avoid premature inheritance** Inheritance needs time to evolve.

**Think interfaces, not Inheritance** Interfaces provide more fluidity in the relationships between classes.

**Overloading functions can become overloading** By using unique names, functions can be more self-describing.

**When in doubt, indirect** Indirection, using either methods or data, adds flexibility.

Source: [Prefactoring: Extreme Abstraction, Extreme Separation, Extreme Readability](http://books.google.co.uk/books?id=IwlcMjDiR_cC)
