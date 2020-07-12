---
title: "A step-by-step guide to Object Oriented Programming"
date: "2007-11-17"
tags: [programming,webdev]
---

According to [Marcus Baker](http://www.lastcraft.com/), if you are a PHP developer and new to OOP, it's possible that your story will go something like this:

- You'll learn the syntax and build an object or two.
- You'll try it in a live project.
- You will realise that you are getting no real advantage, and you'll post your code on the forums.
- People will tell you that your objects are just "smart data", and that you should "think in objects".
- You'll start to build some top level objects, but get confused. Very confused.
- One late project and several rewrites later, you'll come to the conclusion that this OO stuff is a waste of time and go back to procedural.
- You'll repeat the whole sequence six months later.
- ...and again six months after that, but this time you won't listen to advice.
- A year after that, you'll "get it".
- You'll then post lots of helpful advice on forums to other OO beginners suggesting that you "think in objects"...

For those who want to... "think in objects";)

**S****tep-by-step guide to object oriented programming** - By [Marcus Baker](http://www.lastcraft.com/)

1) Learn the syntax by building some small, low level objects. You probably think you know the syntax already, you just read it right? You need to become super comfortable. Don't worry too much about public/protected/private (and don't ever use final), but use everything else.

Say you want to search a table of people and add/delete a row. Where you would have gone...

```

function add_person_to_table(...) { ... }
function delete_person(...) { ... }
```

Try...

```

class People {
    function add(...) { ... }
    function delete(...) { ... }
}
```

This is using classes just as modules. That's fine for now. Do this "bottom up". That is, with the low level mechanical stuff first. You won't yet know how to design, so going "top down" is a non-starter. You won't win much yet, but it will give you a working baseline.

2) Embrace "smart data" if you think it helps. For example, you have an array of arrays. Let's say it's a list of rows from a DB query. Wrap it in a class...

```

class ResultSet {
    function __construct($all_rows) { ... }
    function getRow($i) { ... }
    function getField($i, $field) { ... }
    ...
}
```

Now you can go...

```

$result->getField($i, 'name'); 
```

...instead of...

```

$result[$i]['name']; 
```

Purist will say this is wrong, and you should think in objects. They are right, but it's a step stone, and it's code you can use straight away.

As your curious, here is a sneak peek...

```

class Result {
    function __construct($result_handle) { ... }
    function next() { ... }
} 
```

3) Learn how exceptions work. Error handling in OO can be tricky without them. It's also a nice easy way to break out of the machine view, and you can use them in procedures too.

4) Use some OO libraries to get more comfortable.

5) After a few weeks/months you will want to group the little methods into larger ones, and also pass objects from method to method. It's time to read the "Refactoring" book by Martin Fowler. The technique of refactoring is the one that allows you to experiment at low cost. It's also in this book that you will come across "unit testing".

6) Try unit testing, and later TDD. Paired with refactoring, and backed up by version control, your whole coding style will become more fluid. This new programming style, when coupled with OO, is very powerful. It amplifies the good stuff in OO, and so will allow you to see it all more clearly.

7) Can you swap a component in one line of code? See if you can find a candidate class and then actually try it. If you get stuck, and post to the forums, everyone will start to tell you about some design pattern you should use. These are ways of factoring the code to make different types of changes possible. There are a lot of them, but they are not mysterious, and by this point you will be using them already without realising.

8) Building a fully OO app. is a good thing to do right now. It won't be "reusable", but it will work once. The main thing is that you are close to your earlier productivity with procedures. By now, that was a year ago, but then you were ceiling'ed. You couldn't contemplate building an application larger than you could hold in your head. That ceiling will now start to rise.

9) Once you've got Strategy and Iterator patterns, and a couple of others, time to start reading again. Read lots on patterns and then...forget it. You want the general ideas absorbed into your subconscious, and you want to know what problems can be solved. You don't want to distort your designs by trying hard to include them. Occasionally you want to talk about them and look them up. Then the naming is useful.

10) By now your entire apps. are probably in objects. You're probably concerned that your code is still not "reusable". Don't worry. Nothing ever is.

11) Once you are patterned out, and frankly bored, read some higher level design books such as "Object Design" by Rebecca Wirfs-Brock. Try CRC cards. They're a great way to think in objects.

12) After that you can answer forum posts telling people to "think in objects".

This post was extracted from [SitePoint's](http://www.sitepoint.com/) forum.
