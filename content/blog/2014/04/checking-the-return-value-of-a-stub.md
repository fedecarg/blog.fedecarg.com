---
title: "TDD: Checking the return value of a Stub"
date: "2014-04-15"
tags: [agile-development,java,programming,webdev]
---

State verification is used to ensure that after a method is run, the returned value of the SUT is as expected. Of course, you may need to use Stubs on a test double or a real object to tell the object to return a value in response to a given message.

In Java, you declare a method's return type in its method declaration, this means that the type of the return value must match the declared return type or otherwise you will get a compiler error. In PHP, for example, you dynamically type the return value within the body of the method. This means that PHP mocking libraries cannot check the type of the return value and provide guarantees about what is being verified.

This leads to the awkward situation where a refactoring may change the SUT behaviour and leave a stub broken but with passing tests. For example, consider the following:

Developer (A) creates 2 classes, Presenter and Collaborator:

```
class Presenter
{
    protected $collaborator;

    public function __construct(Collaborator $obj)
    {
        $this->collaborator = $obj;
    }

    public function doSomething()
    {
        $limit = 1;
        $stories = $this->collaborator->getStories($limit);
        // ...
        return $stories;
    }
}

class Collaborator
{
    public function getStories($limit)
    {
        return array();
    }
}
```

Then writes a test case:

```
class PresenterTest extends PHPUnit_Framework_TestCase
{
    // Behaviour verification
    public function testBehaviour()
    {
        $mock = $this->getMock('Collaborator', array('getStories'));
        $mock->expects($this->once())
            ->method('getStories')
            ->with(
                $this->logicalAnd(
                    $this->equalTo(1), $this->isType('integer')
                )
            );

        $presenter = new Presenter($mock);
        $presenter->doSomething();
    }

    // State verification
    public function testState()
    {
        $stub = $this->getMock('Collaborator', array('getStories'));
        $stub->expects($this->once())
            ->method('getStories')
            ->will($this->returnValue(array()));

        $presenter = new Presenter($stub);
        $data = $presenter->doSomething();

        $this->assertEquals(array(), $data);
    }
}
```

The Developer (A) uses a mock to verify the behaviour (a mockist practitioner) and a stub to verify the method worked correctly. The first test asserts that the expectation is met and the second one that the given condition is true. Finally, the Developer runs and watches all of the tests pass. Great!

The next day Developer (B) decides to makes some changes to the Collaborator class and return NULL if there are no stories:

```
class Collaborator
{
    public function getStories($limit)
    {
        $stories = array();
        if (count($stories) < 1) {
            return;
        }

        return $stories;
    }
}
```

The implementation of the method-under-test changed, it now returns a different data type, null instead of array. This means that our second test should fail, but it doesn't. The test still asserts that the given condition is true, even though the return type is different. This is a problem. It means that our second test is unable to verify the correct state of the SUT (and its collaborator).

This is because most PHP mocking libraries are heavily influenced by Java (PHPUnit was originally a port of JUnit), and Java doesn't have this problem. In PHP, the method's return type is not a required elements of a method declaration, so developers can define it at run time and return whatever type they want.

### The solution

You can use DocBlock annotations to make sure the data type of the returned value matches the one defined in the DocBlock. For this to work you need to set the return value using [ReturnValue](https://gist.github.com/fedecarg/9997430) instead of PHPUnit\_Framework\_MockObject\_Stub\_Return. For example:

```
class PresenterTest extends PHPUnit_Framework_TestCase
{
    // State verification
    public function testState()
    {
        $stub = $this->getMock('Collaborator', array('getStories'));
        $stub->expects($this->once())
            ->method('getStories')
            ->will(new ReturnValue(array()));

        $presenter = new Presenter($stub);
        $data = $presenter->doSomething();

        $this->assertEquals(array(), $data);
    }
}
```

Now if you run the test it fails with the following error message:

```
PHPUnit_Framework_Exception: Invalid method declaration; return type required
```

The test also fails if the returned type doesn't match the expected one defined in the DocBlock:

```
class Collaborator
{
    /**
     * @return int
     */
    public function getStories($limit)
    {
        // ...
    }
}
```

Error message:

```
PHPUnit_Framework_Exception: array does not match expected type "int"
```

Or, if you specify more than one data type in the DocBlock:

```
class Collaborator
{
    /**
     * @return array|null
     */
    public function getStories($limit)
    {
        // ...
    }
}
```

Error message:

```
PHPUnit_Framework_Exception: getStories cannot return more than one type, 2 given (array, null)
```

This solution is not perfect but should work in most cases.
