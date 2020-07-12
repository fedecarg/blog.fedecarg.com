---
title: "JavaScript: Retrieve and paginate JSON-encoded data"
date: "2011-10-09"
tags: [javascript,programming]
---

I've created a [jQuery plugin](https://github.com/fedecarg/jquery-paginate) that allows you to retrieve a large data set in JSON format from a server script and load the data into a list or table with client side pagination enabled. To use this plugin you need to:

Include jquery.min.js and jquery.paginate.min.js in your document:

```
http://js/jquery.min.js
http://js/jquery.paginate.min.js
```

Include a small css to skin the navigation links:

```
<style type="text/css">
a.disabled {
    text-decoration: none;
    color: black;
    cursor: default;
}
</style>
```

Define an ID on the element you want to paginate, for example: "listitems". If you have a more than 10 child elements and you want to avoid displaying them before the javascript is executed, you can set the element as hidden by default:

```
<ul id="listitems" style="display:none"></ul>
```

Place a div in the place you want to display the navigation links:

```

    « Previous
    Next »
```

Finally, include an initialization script at the bottom of your page like this:

```

$(document).ready(function() {
    $.getJSON('data.json', function(data) {
        var items = [];
        $.each(data.items, function(i, item) {
            items.push('' + item + '');
        });
        $('#listitems').append(items.join(''));
        $('#listitems').paginate({itemsPerPage: 5});
    });
});
```

You can fork the code on [GitHub](https://github.com/fedecarg/jquery-paginate) or [download it](https://github.com/fedecarg/jquery-paginate/zipball/master).
