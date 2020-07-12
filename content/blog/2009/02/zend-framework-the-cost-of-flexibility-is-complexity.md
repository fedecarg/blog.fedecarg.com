---
title: "Zend Framework: The Cost of Flexibility is Complexity"
date: "2009-02-22"
tags: [frameworks,programming,software-architecture,webdev]
---

The Zend Framework is a very flexible system, in fact, it can be used to build practically anything. But, the problem with using a flexible system is that flexibility costs. And the cost of flexibility is complexity.

[Martin Fowler](http://www.artima.com/intv/flexplex2.html) wrote:

> Every time you put extra stuff into your code to make it more flexible, you are usually adding more complexity. If your guess about the flexibility needs of your software is correct, then you are ahead of the game. You've gained. But if you get it wrong, you've only added complexity that makes it more difficult to change your software.

Don't assume that just because you're using an object-oriented framework you are writing reusable and maintainable code. You are just structuring your spaghetti code. As [Paul Graham](http://www.paulgraham.com/hundred.html) put it: "Object-oriented programming offers a sustainable way to write spaghetti code". The main problem with flexibility is that most developers give up trying to understand. I don't blame them, no one likes dealing with complexity. However, flexibility is sometimes required and generally always desirable. But, sometimes we forget that frameworks exist to help us solve or address complex issues, not to fix bad design decisions for us. So if your code is difficult to understand or unit test, then you are probably doing something wrong.

### Just because you can, doesn’t mean you should

What's the best way to render a view script in the Zend Framework? Think, don't give up yet. Is it enabling or disabling the ViewRenderer action helper? Remember, the framework allows you to shape the Zend\_Controller\_Action class to your application's needs.

The manual clearly states that:

> By default, the front controller enables the ViewRenderer action helper. This helper takes care of injecting the view object into the controller, as well as automatically rendering views.

However, no one stops you from doing it manually:

> Zend\_Controller\_Action provides a rudimentary and **flexible** mechanism for view integration. Two methods accomplish this, initView() and render(); the former method lazy-loads the $view public property, and the latter renders a view based on the current requested action, using the directory hierarchy to determine the script path.

### Developer A

```
class IndexController extends Zend_Controller_Action
{
    public function searchAction()
    {
        $query = $this->_getParam('q', null);
        if (null === $query) {
            $this->view->error = 'Invalid query';
            return;
        }

        $this->view->query = $query;
        if (is_numeric($query)) {
            $this->setUserDetailsById($query);
        } else {
            $this->setUserDetailsByName($query);
        }
    }

    public function setUserDetailsByName($name)
    {
        $row = $this->findUserByName($name);
        $this->view->userId = $row['id'];
        $this->view->userName = $row['name'];
    }

    public function setUserDetailsById($id)
    {
        $row = $this->findUserById($id);
        $this->view->userId = $row['id'];
        $this->view->userName = $row['name'];
    }
}
```

### Developer B

```
class IndexController extends Zend_Controller_Action
{
    protected $_viewParams = array(
        'error'    => null,
        'query'    => null,
        'userId'   => null,
        'userName' => null,
    );

    public function init()
    {
        $this->_helper->removeHelper('viewRenderer');
    }

    public function searchAction()
    {
        $query = $this->_getParam('q', null);
        if (null === $query) {
            $this->_viewParams['error'] = 'Invalid query';
            $this->renderView('search');
            return false;
        }

        return $this->displayUserDetails($query);
    }

    public function displayUserDetails($query)
    {
        if (is_numeric($query)) {
            $row = $this->getUserDetailsById($query);
        } else {
            $row = $this->getUserDetailsByName($query);
        }

        if (!$row) {
            $this->_viewParams['error'] = 'User not found';
            $this->renderView('search');
            return false;
        }

        foreach ($this->_viewParams as $key => $value) {
            if (!array_key_exists($key, $row) {
                continue;
            }
            $this->_viewParams[$key] = $row[$key];
        }

        $this->_viewParams['query'] = $query;
        $this->renderView('search');

        return true;
    }

    public function renderView($template)
    {
        $view = $this->initView();
        foreach ($this->_viewParams as $key => $value) {
            $view->$key = $value;
        }
        $this->render($template);
    }

    public function getUserDetailsByName($name)
    {
        return $this->findUserByName($name);
    }

    public function getUserDetailsById($id)
    {
        return $this->findUserById($id);
    }
}
```

Same problem, different solutions.

Now, guess who is doing test-driven development, has all his code unit tested, puts commonly used code into libraries, abstracts out common patterns of behaviour, writes his code using the top-down fashion and uses the principle of “least astonishment”.

Developer A or B?

### Conclusion

Many requirements lie in the future and are unknowable at the time our application is designed and built. To avoid burdensome maintenance costs developers must therefore rely on a system's ability to change gracefully its flexibility. The combination of flexibility and smart design can reduce the maintenance burden. It’s up to us, the developers, to change our thinking when using a flexible framework. Like I said before, frameworks exist to help us solve complex issues, not to make decisions for us.
