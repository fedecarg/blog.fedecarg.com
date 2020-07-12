---
title: "Running Quercus in Jetty Web Server"
date: "2009-01-04"
tags: [java,linux,webdev]
---

Jetty Web server can be invoked and installed as a stand alone application server. It has a flexible component based architecture that allows it to be easily deployed and integrated in a diverse range of instances. The project is supported by a growing community and a team with a history of being responsive to innovations and changing requirements. More info [here](http://www.webtide.com/choose/jetty.jsp).

### Installing Jetty

First you need to download Jetty. It's distributed as a platform independent zip file containing source, javadocs and binaries. The most recent distro can be downloaded from [Codehaus](http://dist.codehaus.org/jetty/):

```
$ wget http://dist.codehaus.org/jetty/jetty-6.1.14/jetty-6.1.14.zip
$ unzip jetty-6.1.14.zip
$ cp -R jetty-6.1.14 /opt/
$ cd /opt
$ ln -s /opt/jetty-6.1.14 jetty
```

Problems installing Jetty? More info [here](http://docs.codehaus.org/display/JETTY/Installing+Jetty-6.1.x).

### Running Jetty

Running jetty is as simple as going to your jetty installation directory and typing:

```
$ cd /opt/jetty
$ java -jar start.jar etc/jetty.xml
```

This will start jetty and deploy a demo application available at:

```
http://localhost:8080/test
```

That's it. Now stop Jetty with cntrl-c in the same terminal window as you started it.

### Installing Quercus

[Quercus](http://kewnode.wordpress.com/2008/08/09/php-implemented-in-100-java/) is a complete implementation of the PHP language and libraries in Java. It gives both Java and PHP developers a fast, safe, and powerful alternative to the standard PHP interpreter. Quercus is available for download as a WAR file which can be easily deployed on Jetty:

```
$ wget -P ~/quercus http://quercus.caucho.com/download/quercus-3.2.1.war
$ jar xf ~/quercus/quercus-3.2.1.war
```

Unpack the WAR file and copy all the jars to Jetty's global library directory:

```
$ cp ~/quercus/WEB-INF/lib/* /opt/jetty/lib
```

### Configuring Jetty

Edit the web.xml file:

```
$ vi /opt/jetty/webapps/test/WEB-INF/web.xml
```

Add the following between the web-app tags:

```
<servlet>
    <servlet-name>Quercus Servlet</servlet-name>
    <servlet-class>com.caucho.quercus.servlet.QuercusServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>Quercus Servlet</servlet-name>
    <url-pattern>*.php</url-pattern>
</servlet-mapping>
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.php</welcome-file>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```

Create a PHP file inside the test application:

```
$ cat /opt/jetty/webapps/test/index.php
<?php phpinfo(); ?>
```

This file will be available at:

```
http://localhost:8080/index.php
```

It works! You are now ready to:

**Instantiate objects by class name**

```
<?php
$a = new Java("java.util.Date", 123);
print $a->time;
```

**Import classes**

```
<?php
import java.util.Date;

$a = new Date(123);
print $a->time;
```

**Call Java methods**

```
<?php
import java.util.Date;

$a = new Date(123);
print $a->getTime();
print $a->setTime(456);

print $a->time;
$a->time = 456;
```

And much, [much more](http://www.caucho.com/resin/doc/quercus.xtp).
