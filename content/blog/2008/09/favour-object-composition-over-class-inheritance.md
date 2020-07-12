---
title: "Favour object composition over class inheritance"
date: "2008-09-04"
tags: [design-patterns,programming,webdev]
---

What does "favour composition over inheritance" mean, and why is it a good thing to do?

Object composition and inheritance are two techniques for reusing functionality in object-oriented systems. In general, object composition should be favoured over inheritance. It promotes smaller, more focused classes and smaller inheritance hierarchies.

Troels Knak-Nielsen [wrote](http://www.sitepoint.com/forums/showpost.php?p=3945208&postcount=5):

Class inheritance is a mix of two concepts. The extending class inherits the parents implementation (functions/methods) and it inherits the parents type. In a statically typed language, the latter is fairly important, since you can't freely mix types. So if some method expects an argument of a given type, you might use inheritance to satisfy this. In a dynamically typed language that is a non-issue. You can simply implement the expected behaviour and that's all there is to it. If you need a more explicit contract, you can document it or - since PHP has sort of a middle-way on this matter - you could use a statically typed interface (Eg. implements Person, rather than extends Person) to do the same thing. Since PHP is dynamically typed, this (slightly more verbose and restrictive) solution is purely optional. You can just use an implicit contract (duck typing).

The other use of class inheritance is to reuse implementation. If your abstract class Person is extended by a subclass Employer, you would have access to the same code in Employer as you do in Person. You could achieve code-reuse with composition as well, but it takes a bit more work. Employer would have to implement a wrapper that delegates control to a Person instance in this case. Eg.:

```
class Person {
  function sayHello() {
    echo "Hello, World!";
  }
}
class Employer {
  protected $person;
  function __construct() {
    $this->person = new Person();
  }
  function sayHello() {
    $this->person->sayHello();
  }
}
```

rather than:

```
class Person {
  function sayHello() {
    echo "Hello, World!";
  }
}
class Employer extends Person {}
```

As you can see, slightly more work to do, which is why people often use inheritance in these cases. The cost however, is that the Person-Employer relationship is now set in stone; It can't be changed or intercepted at runtime. There is also the matter of clarity. While the compositional code is more verbose, it is also very clear about what it does. You can look at the code and know what it does. With the inheritance version, you need to look at the superclass to find out what it does. Some times there are multiple levels of inheritance, making you trace up and down the chain to figure out exactly what code is available in the concrete class. Finally, there is the problem of multiple inheritance. In PHP, you can't. You only have one shot at inheritance, so if you want to reuse code from two places, well, you're out of luck.

Source: [SitePoint Forums](http://www.sitepoint.com/forums/showpost.php?p=3945208&postcount=5)
