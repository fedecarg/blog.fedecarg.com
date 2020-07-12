---
title: "Testing Zend Framework Action Controllers With Mocks"
date: "2009-11-01"
tags: [agile-development,frameworks,webdev]
---

In this post I'll demonstrate a unit test technique for testing Zend Framework Action Controllers using Mock Objects. Unit testing controllers independently has a number of advantages:

1. You can develop controllers test-first (TDD).
2. It allows you to develop and test all of your controller code before developing any of the view scripts.
3. It helps you quickly identify problems in the controller, rather than problems in one of the combination of Model, View and Controller.

The Action Controller I'm going to test has only one method, profileAction():

**tests/application/controllers/UserController.php**

```
class UserController extends Zend_Controller_Action
{
    public function profileAction()
    {
        $this->view->userId = $this->_getParam('user_id');
        return $this->render();
    }
}
```

**tests/application/ControllerTestCase.php**

```
class ControllerTestCase extends Zend_Test_PHPUnit_ControllerTestCase
{
    public $application;

    public function setUp()
    {
        $this->application = new Zend_Application(
            APPLICATION_ENV,
            APPLICATION_PATH . '/config/application.ini'
        );

        $this->bootstrap = array($this, 'bootstrap');
        parent::setUp();
    }

    public function tearDown()
    {
        Zend_Controller_Front::getInstance()->resetInstance();

        $this->resetRequest();
        $this->resetResponse();

        $this->request->setPost(array());
        $this->request->setQuery(array());
    }

    public function bootstrap()
    {
        $this->application->bootstrap();
    }
}
```

**tests/application/controllers/UserControllerTest.php**

```
require_once TESTS_PATH . '/application/ControllerTestCase.php';
require_once APPLICATION_PATH . '/controllers/UserController.php';

class UserControllerTest extends ControllerTestCase
{
    public function testStubRenderMethodCall()
    {
        $request = $this->getRequest()
            ->setRequestUri('/user/profile/1')
            ->setParams(array('user_id'=>1))
            ->setPathInfo(null);

        $response = $this->getResponse();

        $this->getFrontController()
            ->setRequest($request)
            ->setResponse($response)
            ->throwExceptions(true)
            ->returnResponse(false);

        $controller = $this->getMock(
            'UserController',
            array('render'),
            array($request, $response, $request->getParams())
        );
        $controller->expects($this->once())
                 ->method('render')
                 ->will($this->returnValue(true));

        $this->assertTrue($controller->profileAction());
        $this->assertTrue($controller->view->user_id == 1);
    }
}
```

You can go further making both the tests and the implementation more sophisticated. The main point is that you can build and test a controller in a way that doesn't require a view script to be written to do so.

### Zend Framework Known Issues

By default Zend\_Test\_PHPUnit\_ControllerTestCase sets the redirector exit value to false, leading to unexpected behavior when unit testing your code. For that reason, make sure you always add a return statement after calling a utility method:

```
class UserController extends Zend_Controller_Action
{
    public function profileAction()
    {
        if (null == $this->_getParam('user_id', null) {
            return $this->_redirect('/');
        }
        return $this->render();
    }
}
```

If you want the Front Controller to throw exceptions, you have no other choice than to overwrite the dispatch method and pass a boolean TRUE to the throwExceptions() method:

```
class ControllerTestCase extends Zend_Test_PHPUnit_ControllerTestCase
{
    ...

    public function dispatch($url = null)
    {
        // redirector should not exit
        $redirector = Zend_Controller_Action_HelperBroker::getStaticHelper('redirector');
        $redirector->setExit(false);

        // json helper should not exit
        $json = Zend_Controller_Action_HelperBroker::getStaticHelper('json');
        $json->suppressExit = true;

        $request = $this->getRequest();
        if (null !== $url) {
            $request->setRequestUri($url);
        }
        $request->setPathInfo(null);

        $this->getFrontController()
             ->setRequest($request)
             ->setResponse($this->getResponse())
             ->throwExceptions(true)
             ->returnResponse(false);

        $this->getFrontController()->dispatch();
    }

    ...
}
```

The Dispatcher not only [violates the DRY principle](http://framework.zend.com/wiki/display/ZFPROP/Zend_Controller_Directory+-+Federico+Cargnelutti) but also suffers from amnesia. The problem is that it doesn't store the instance of the Action Controller, instead, it destroys it (Zend\_Controller\_Dispatcher\_Standard Line 305). You can easily get around this issue by extending the standard dispatcher and overwriting the dispatch() method:

```
class Zf_Controller_Dispatcher_Standard extends Zend_Controller_Dispatcher_Standard
{
    ...

    public function dispatch($url = null)
    {
        ...
        Zend_Registry::set('Zend_Controller_Action', $controller);

        // Destroy the page controller instance and reflection objects
        $controller = null;
    }
```

This will allow you to access the view object after dispatching the request:

```
class ExampleControllerTest extends ControllerTestCase
{
    public function testDefaultActionRendersViewObject()
    {
        $this->dispatch('/');

        $controller = Zend_Registry::get('Zend_Controller_Action');

        $this->assertEquals('ExampleController', get_class($controller));
        $this->assertTrue(isset($controller->view));
    }
```

### Links

[PHPUnit: Testing Zend Framework Controllers](http://blog.fedecarg.com/2008/12/27/phpunit-testing-zend-framework-controllers/) [PHPUnit: Mock Objects](http://www.phpunit.de/manual/3.4/en/test-doubles.html)
