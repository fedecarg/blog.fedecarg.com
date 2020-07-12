---
title: "Polymorphism"
date: "2007-03-30"
tags: [design-patterns,programming,webdev]
---

The subject of polymorphism is probably the most important in OOP. Using classes and inheritance makes it easy to describe a real-life situation as opposed to just a collection of functions and data. They also make it much easier to grow projects by reusing code mainly via inheritance. Also, to write robust and extensible code, you usually want to have as few as possible flow-control statements (such as if() statements). Polymorphism answers all these needs and more.

Consider the following code:

```

<?php

class Cat
{
	public function miau()
	{
		print "miau";
	}
}

class Dog
{
	public function wuff()
	{
		print "wuff";
	}

}

function printTheRightSound($obj)
{
	if ($obj instanceof Cat) {
		$obj->miau();
	} else if ($obj instanceof Dog) {
		$obj->wuff();
	} else {
		print "Error: Passed wrong kind of object";
	}

	print "n";
}

printTheRightSound(new Cat());
printTheRightSound(new Dog());

?>
```

The output is: miau wuff

You can easily see that this example is not extensible. Say that you want to extend it by adding the sounds of three more animals. You would have to add another three else if blocks to printTheRightSound() so you check that the object you have is an instance of one of those new animals, and then you have to add the code to call each sound method.

Polymorphism using inheritance solves this problem. It enables you to inherit from a parent class, inheriting all its methods and properties and thus creating an is-a relationship.

Taking the previous example, we will create a new class called Animal from which all other animal kinds will inherit, thus creating is-a relationships from the specific kinds, such as Dog, to the parent (or ancestor) Animal.

Inheritance is performed by using the extends keyword:

```

class Child extends Parent { ... }
```

This is how you would rewrite the previous example using inheritance:

```

<?php

class Animal
{
	public function makeSound()
	{
		print "Error: This method should be re-implemented in the children";
	}
}

class Cat extends Animal
{
	public function makeSound()
	{
		print "miau";
	}
}

class Dog extends Animal
{
	public function makeSound()
	{
		print "wuff";
	}
}

function printTheRightSound($obj)
{
	if ($obj instanceof Animal) {
		$obj->makeSound();
	} else {
		print "Error: Passed wrong kind of object";
	}

	print "n";
}

printTheRightSound(new Cat());
printTheRightSound(new Dog());

?>
```

The output is: miau wuff

You can see that no matter how many animal types you add to this example, you will not have to make any changes to printTheRightSound() because the instanceof Animal check covers all of them, and the $obj->makeSound() call will do so, too.
