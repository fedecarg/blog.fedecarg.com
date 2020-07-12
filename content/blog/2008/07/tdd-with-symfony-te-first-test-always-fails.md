---
title: "TDD with Symfony: The first test always fails"
date: "2008-07-02"
tags: [frameworks,programming,webdev]
---

Symfony is one of the few PHP frameworks that gives you basic tools for starting to write tests. It provides a special object, called sfBrowser, which acts like a browser connected to an application without actually needing a server and without the slowdown of the HTTP transport. It gives access to the core objects of each request (the request, session, context, and response objects). Symfony also provides an extension of this class called sfTestBrowser, designed especially for functional tests, which has all the abilities of the sfBrowser object plus some smart assert methods.

For example, every time you generate a module skeleton Symfony creates a simple functional test for this module. The test makes a request to the default action of the module and checks the response status code, the module and action calculated by the routing system, and the presence of a certain sentence in the response content.

### Browser

For a foobar module, the generated foobarActionsTest.php file looks like:

```
include(dirname(__FILE__).'/../../bootstrap/functional.php');

// Create a new test browser
$browser = new sfTestBrowser();
$browser->initialize();

$browser->
  get('/foobar/index')->
  isStatusCode(200)->
  isRequestParameter('module', 'foobar')->
  isRequestParameter('action', 'index')->
  checkResponseElement('body', '!/This is a temporary page/');
```

### Console

And you can launch the functional test from the command line:

```
> symfony test-functional frontend foobarActions

# get /comment/index
ok 1 - status code is 200
ok 2 - request parameter module is foobar
ok 3 - request parameter action is index
not ok 4 - response selector body does not match regex /This is a temporary page/
# Looks like you failed 1 tests of 4.
1..4
```

### Test-driven development

The generated functional tests for a new module don't pass by default. As long as you don't modify the index action, the tests for this module will fail, and this guarantees that you cannot pass all tests with an unfinished module.

Now, that's what I call TDD!

In test-driven development, each new feature begins with writing a test. This test must inevitably fail because it is written before the feature has been implemented.
