---
title: "Increase speed and reduce bandwidth usage"
date: "2009-07-06"
tags: [frameworks,linux,open-source,programming,webdev]
---

Apache's mod\_deflate module provides the DEFLATE output filter that allows output from your server to be compressed before being sent to the client over the network.

There are two ways of enabling gzip compression:

1. Using Apache's mod\_deflate
2. Using output buffering

Encoding the output and setting the appropriate headers manually makes the code more portable. Keep in mind that there are hundreds of Linux distributions, each slightly different to significantly different. To allow portability the application should not make assumptions about the OS or config involved.

### Using Apache

**1\. Enable mod\_deflate**

Debian/Ubuntu:

```
$ a2enmod deflate
$ /etc/init.d/apache2 force-reload
```

**2\. Configure mode\_deflate**

```
$ nano /etc/apache2/mods-enabled/deflate.conf

#
# mod_deflate configuration
#
<IfModule mod_deflate.c>
 AddOutputFilterByType DEFLATE text/plain
 AddOutputFilterByType DEFLATE text/html
 AddOutputFilterByType DEFLATE text/xml
 AddOutputFilterByType DEFLATE text/css
 AddOutputFilterByType DEFLATE application/xml
 AddOutputFilterByType DEFLATE application/xhtml+xml
 AddOutputFilterByType DEFLATE application/rss+xml
 AddOutputFilterByType DEFLATE application/javascript
 AddOutputFilterByType DEFLATE application/x-javascript

 DeflateCompressionLevel 9

 BrowserMatch ^Mozilla/4 gzip-only-text/html
 BrowserMatch ^Mozilla/4\.0[678] no-gzip
 BrowserMatch \bMSIE !no-gzip !gzip-only-text/html

 DeflateFilterNote Input instream
 DeflateFilterNote Output outstream
 DeflateFilterNote Ratio ratio
</IfModule>
```

### Using output buffering

Create a gzip compressed string in your bootstrap file:

```
try {
    $frontController = Zend_Controller_Front::getInstance();
    if (@strpos($_SERVER['HTTP_ACCEPT_ENCODING'], 'gzip') !== false) {
        ob_start();
        $frontController->dispatch();
        $output = gzencode(ob_get_contents(), 9);
        ob_end_clean();
        header('Content-Encoding: gzip');
        echo $output;
    } else {
        $frontController->dispatch();
    }
} catch (Exeption $e) {
    if (Zend_Registry::isRegistered('Zend_Log')) {
        Zend_Registry::get('Zend_Log')->err($e->getMessage());
    }
    $message = $e->getMessage() . "\n\n" . $e->getTraceAsString();
    /* trigger event */
}
```

### Reference

[Use mod\_deflate to compress Web content delivered by Apache](http://www.g-loaded.eu/2008/05/10/use-mod_deflate-to-compress-web-content-delivered-by-apache/)
