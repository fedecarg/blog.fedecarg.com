---
title: "JavaScript: Asynchronous Script Loading and Lazy Loading"
date: "2011-07-12"
tags: [design-patterns,javascript,programming,software-architecture]
---

Most of the time remote scripts are included at the end of an HTML document, right before the closing body tag. This is because browsers are single threaded and when they encounter a script tag, they halt any other processes until they download and parse the script. By including scripts at the end, you allow the browser to download and render all page elements, style sheets and images without any unnecessary delay. Also, if the browser renders the page before executing any script, you know that all page elements are already available to retrieve.

However, websites like Facebook for example, use a more advanced technique. They include scripts dynamically via DOM methods. This technique, which I'll briefly explain here, is known as "Asynchronous Script Loading".

Lets take a look at the script that Facebook uses to download its JS library:

```
(function () {
    var e = document.createElement('script');
    e.src = 'http://connect.facebook.net/en_US/all.js';
    e.async = true;
    document.getElementById('fb-root').appendChild(e);
}());
```

When you dynamically append a script to a page, the browser does not halt other processes, so it continues rendering page elements and downloading resources. The best place to put this code is right after the opening body tag. This allows Facebook initialization to happen in parallel with the initialization on the rest of the page.

Facebook also makes non-blocking loading of the script easy to use by providing the fbAsyncInit hook. If this global function is defined, it will be executed when the library is loaded.

```
window.fbAsyncInit = function () {
    FB.init({
        appId: 'YOUR APP ID',
        status: true,
        cookie: true,
        xfbml: true
    });
};
```

Once the library has loaded, Facebook checks the value of window.fbAsyncInit.hasRun and if it's false it makes a call to the fbAsyncInit function:

```
if (window.fbAsyncInit && !window.fbAsyncInit.hasRun) {
    window.fbAsyncInit.hasRun = true;
    fbAsyncInit();
}
```

Now, what if you want to load multiple files asynchronously, or you need to include a small amount of code at page load and then download other scripts only when needed? Loading scripts on demand is called "Lazy Loading". There are many libraries that exist specifically for this purpose, however, you only need a few lines of JavaScript to do this.

Here is an example:

```
$L = function (c, d) {
    for (var b = c.length, e = b, f = function () {
            if (!(this.readyState
            		&& this.readyState !== "complete"
            		&& this.readyState !== "loaded")) {
                this.onload = this.onreadystatechange = null;
                --e || d()
            }
        }, g = document.getElementsByTagName("head")[0], i = function (h) {
            var a = document.createElement("script");
            a.async = true;
            a.src = h;
            a.onload = a.onreadystatechange = f;
            g.appendChild(a)
        }; b;) i(c[--b])
};
```

The best place to put this code is inside the head tag. You can then use the $L function to asynchronously load your scripts on demand. $L takes two arguments: an array (c) and a callback function (d).

```
var scripts = [];
scripts[0] = 'http://www.google-analytics.com/ga.js';
scripts[1] = 'http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.js';

$L(scripts, function () {
    console.log("ga and jquery scripts loaded");
});

$L(['http://connect.facebook.net/en_US/all.js'], function () {
    console.log("facebook script loaded");
    window.fbAsyncInit.hasRun = true;
    FB.init({
        appId: 'YOUR APP ID',
        status: true,
        cookie: true,
        xfbml: true
    });
});
```

You can see this script in action [here](http://fbrell.com/auth/account-info) (right click -> view page source).
