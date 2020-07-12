---
title: "Intercepting class method invocations using metaclass programming in Python"
date: "2015-01-08"
tags: [programming,python]
---

In Ruby, objects have a handy method called method\_missing which allows one to handle method calls for methods that have not been defined. Most examples out there explain how to implement this in Python using \_\_getattr\_\_, however, none of them (honestly, none) explain how to intercept class method (@classmethod) invocations using \_\_metaclass\_\_.

And that's why I wrote this post.

The function type is the built-in metaclass Python uses, it not only lets you know the type of an object, but also create classes on the fly. When you write, for example: class Example(object) the class object Example is not created in memory straight away. Python looks for the \_\_metaclass\_\_ attribute in the class definition and if it finds it, it uses it to create the object class Example. If it doesn't, it uses type to create the class. The main purpose of a metaclass is to change the class automatically, when it's created.

Here's an example of how to use metaclass programming to intercept class method calls similar to the method\_missing technique in Ruby:

```
class ClassMethodInterceptor(type):

    def __getattr__(cls, name):
        return lambda *args, **kwargs: \
                   cls.static_method_missing(name, *args, **kwargs)

    def static_method_missing(cls, method_name, *args, **kwargs):
        e = "type object 'static.%s' has no attribute '%s'" \
            % (cls.__name__, method_name)
        raise AttributeError(e)

class Example(object):

    __metaclass__ = ClassMethodInterceptor

    def __getattr__(self, name):
        return lambda *args, **kwargs: \
                   self.method_missing(name, *args, **kwargs)

    def method_missing(self, method_name, *args, **kwargs):
        e = "type object '%s' has no attribute '%s'" \
            % (self.__class__.__name__, method_name)
        raise AttributeError(e)

    @classmethod
    def static(cls):
        print 'static.%s' % cls.__name__

    def instance(self):
        print self.__class__.__name__
```

Console:

```
>>> Example.static()
static.Example
>>> Example.foo()
Traceback (most recent call last):
...
  File "example.py", line 12, in static_method_missing
    raise AttributeError(e)
AttributeError: type object 'static.Example' has no attribute 'foo'
>>> e = Example()
>>> e.instance()
Example
>>> e.foo()
Traceback (most recent call last):
...
  File "example.py", line 26, in method_missing
    raise AttributeError(e)

AttributeError: type object 'Example' has no attribute 'foo'
```

If you ever implement something like this, remember that Python doesn't distinguish between methods and attributes the way Ruby does. There is no difference in Python between properties and methods. A method is just a property whose type is instancemethod.
