---
title: "Zend Framework Automatic Dependency Tracking"
date: "2009-02-01"
tags: [frameworks,open-source,programming,webdev]
---

When you develop or deploy an application, dependency tracking is one of the problems you must solve. Keeping track of dependencies for every application you develop is not an easy task. To solve this problem I've created [Zend\_Debug\_Include](http://svn.fedecarg.com/repo/Zend/Debug/Include), a Zend Framework component that supports automatic dependency tracking.

We all agree that dependencies cannot be maintained by hand, it's almost impossible, specially if you are using packages from [Zend](http://framework.zend.com/), [Solar](http://www.solarphp.com/), [Maintainable](http://framework.maintainable.com/) and/or [Zym](http://www.zym-project.com/) (if you don't know any of these frameworks, check them out, they are pretty cool). Every time you add an include/require statement or create an instance of an object, you introduce a new dependency. And that's why tracking internal project dependencies can become complex when using a framework.

The concept behind [Zend\_Debug\_Include](http://svn.fedecarg.com/repo/Zend/Debug/Include) is that the dependencies for each source file are stored in a separate file. If the source file is modified, the file containing that source file's dependencies is rebuilt. This concept enables you to determine run-time dependencies of files using arbitrary components. This solution is also useful if you are deploying your application using Linux packages. But, dependency tracking isn't just useful for deploying applications, it can also be used to evaluate packages. Sometimes packages create unnecessary dependencies and this is something that we need to monitor.

Zend\_Debug\_Include comes with 3 built-in adapters: [File](http://svn.fedecarg.com/repo/Zend/Debug/Include/Adapter/File.php), [Package](http://svn.fedecarg.com/repo/Zend/Debug/Include/Adapter/Package.php) and [Url](http://svn.fedecarg.com/repo/Zend/Debug/Include/Adapter/Url.php).

### Tracking File Dependencies

To track file dependencies you need to create an instance of Zend\_Debug\_Include\_Manager and set the Zend\_Debug\_Include\_Adapter\_File adapter. This needs to happen inside your bootstrapper file, before the Front Controller dispatches the request.

```
$included = new Zend_Debug_Include_Manager();
$included->setAdapter(new Zend_Debug_Include_Adapter_File());
$included->setOutputDir('/var/www/my-app/dependencies');

/* Dispatch request */
$frontController->dispatch();
```

This creates a zf-files.txt in your output directory containing all the files included or required on that request. Every time the Front Controller dispatches a request, Zend\_Debug\_Include checks for new dependencies and adds them to the zf-files.txt file:

```
/var/www/my-app/public/index.php
/var/www/my-app/application/bootstrap.php
/var/www/my-app/library/Zend/Loader.php
/var/www/my-app/library/Zend/Controller/Front.php
/var/www/my-app/library/Zend/Controller/Action/HelperBroker.php
/var/www/my-app/library/Zend/Controller/Action/HelperBroker/PriorityStack.php
/var/www/my-app/library/Zend/Controller/Exception.php
/var/www/my-app/library/Zend/Exception.php
/var/www/my-app/library/Zend/Controller/Plugin/Broker.php
/var/www/my-app/library/Zend/Controller/Plugin/Abstract.php
/var/www/my-app/library/Zend/Controller/Dispatcher/Standard.php
/var/www/my-app/library/Zend/Controller/Dispatcher/Abstract.php
/var/www/my-app/library/Zend/Controller/Dispatcher/Interface.php
/var/www/my-app/library/Zend/Controller/Request/Abstract.php
/var/www/my-app/library/Zend/Controller/Response/Abstract.php
/var/www/my-app/library/Zend/Controller/Router/Route.php
/var/www/my-app/library/Zend/Controller/Router/Route/Abstract.php
/var/www/my-app/library/Zend/Controller/Router/Route/Interface.php
/var/www/my-app/library/Zend/Config.php
... (63 files in total) ...
```

To change the name of the file:

```
$included = new Zend_Debug_Include_Manager();
$included->setOutputDir('/var/www/my-app/dependencies');
$included->setFilename('files.dep');
...
```

### Tracking Package Dependencies

Similar to Zend\_Debug\_Include\_Adapter\_File, but groups all the files into packages.

```
$included = new Zend_Debug_Include_Manager();
$included->setAdapter(new Zend_Debug_Include_Adapter_Package());
$included->setOutputDir('/var/www/my-app/dependencies');
```

The code above creates a zf-packages.txt file and adds the following data:

```
Zend/Loader.php
Zend/Controller
Zend/Exception.php
Zend/Config.php
Zend/Debug
Zend/View.php
Zend/View
Zend/Loader
Zend/Uri.php
Zend/Filter
Zend/Filter.php
```

Now, if you introduce a new dependency, for example, Zend\_Mail:

```
class IndexController extends Zend_Controller_Action
{
    public function indexAction()
    {
        $mail = new Zend_Mail();
        $this->view->message  = 'test';
    }
}
```

The next time the Front Controller dispatches the request and calls the index action, Zend\_Debug\_Include will automatically add the Zend\_Mail package and all its dependencies to the zf-packages.txt file:

```
Zend/Loader.php
Zend/Controller
Zend/Exception.php
Zend/Config.php
Zend/Debug
Zend/View.php
Zend/View
Zend/Loader
Zend/Uri.php
Zend/Filter
Zend/Filter.php
Zend/Mail.php
Zend/Mail
Zend/Mime.php
Zend/Mime
```

You can then use this information to keep track of dependencies and tell your build tool the name of the files and directories you need to copy and package.

### External Dependencies

Here is where everything starts to make sense. Zend\_Debug\_Include allows you to search for external dependencies as well, you just need to tell the Adapter the libraries you are using. For example:

```
$libraries = array('Zend', 'Solar');
$adapter = new Zend_Debug_Include_Adapter_Package($libraries);

$included = new Zend_Debug_Include_Manager();
$included->setAdapter($adapter);
$included->setOutputDir('/var/www/my-app/dependencies');
```

The Solar packages will also be added to the zf-packages.txt file:

```
Zend/Loader.php
Zend/Controller
Zend/Exception.php
Zend/Config.php
Zend/Debug
Zend/View.php
Zend/View
Zend/Loader
Zend/Uri.php
Zend/Filter
Zend/Filter.php
Zend/Mail.php
Zend/Mail
Zend/Mime.php
Zend/Mime
Solar/Base.php
Solar/File.php
Solar/Factory.php
Solar/Sql.php
Solar/Sql
Solar/Cache.php
Solar/Cache
```

### URL Adapter

If you want to create a different file for each request, use the URL adapter instead:

```
$included = new Zend_Debug_Include_Manager();
$included->setAdapter(new Zend_Debug_Include_Adapter_Url());
$included->setOutputDir('/var/www/my-app/dependencies');
```

The URL adapter maps the URL path to a filename. So, if you request the following URI:

```
http://my-app/blog/2009/02/01
```

It creates the file blog\_2009\_02\_01.txt.

### SVN

```
$ cd libraries/Zend
$ svn co http://svn.fedecarg.com/repo/Zend/Debug
```
