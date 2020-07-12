---
title: "Implementing the Delegation Pattern Using Reflection"
date: "2007-11-12"
tags: [design-patterns,programming,webdev]
---

Times arise where a class (One) is supposed to do everything another class (Two) does and more. The preliminary temptation would be for class One to extend class Two , and thereby inheriting all of its functionality. However, there are times when this is the wrong thing to do, either because there isn't a clear semantic is-a relationship between classes One and Two , or class One is already extending another class, and inheritance cannot be used. Under such circumstances, it is useful to use a delegation model (**via the delegation design pattern**), where method calls that class One can't handle are redirected to class Two . In some cases, you may even want to chain a larger number of objects where the first one in the list has highest priority. The following example creates such a delegator called ClassOneDelegator that first checks if the method exists and is accessible in ClassOne ; if not, it tries all other objects that are registered with it. The application can register additional objects that should be delegated to by using the addObject($obj) method. The order of adding the objects is the order of precedence when Class OneDelegator searches for an object that can satisfy the request:

```
class ClassOne {
	function callClassOne() {
		print "In Class One\n";
	}
}

class ClassTwo {
	function callClassTwo() {
		print "In Class Two\n";
	}
}

class ClassOneDelegator {
	private $target;

	function __construct() {
		$this->target[] = new ClassOne();
	}

	function addObject($obj) {
		$this->target[] = $obj;
	}

	function __call($name, $args) {
		foreach ($this->target as $obj) {
			$r = new ReflectionClass($obj);
			if (!$r->hasMethod($name)) {
				continue;
			}

			$method = $r->getMethod($name);
			if ($method->isPublic() && !$method->isAbstract()) {
				return $method->invoke($obj, $args);
			}
		}
	}
}

$obj = new ClassOneDelegator();
$obj->addObject(new ClassTwo());
$obj->callClassOne();
$obj->callClassTwo();
```

Running this code results in the following output:

```
In Class One
In Class Two
```

You can see that this example uses the previously described feature of overloading method calls using the special \_\_call() method. After the call is intercepted, \_\_call() uses the reflection API to search for an object that can satisfy the request. Such an object is defined as an object that has a method with the same name, which is publicly accessible and is not an abstract method.

Currently, the code does nothing if no satisfying function is found. You may want to call ClassOne by default, so that you make PHP error out with a nice error message, and in case ClassOne has itself defined a \_\_call() method, it would be called. It is up to you to implement the default case in a way that suits your needs.

PHP 5 Power Programming. Sample Chapter is provided courtesy of Prentice Hall PTR.[](http://www.informit.com/store/product.aspx?isbn=013147149X)

[Buy this book.](http://www.informit.com/store/product.aspx?isbn=013147149X)
