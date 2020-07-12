---
title: "Unit Testing: Rules of Thumb"
date: "2008-06-17"
tags: [agile-development,tools]
---

When it comes to testing, Cedric Beust (co-author of "Next Generation Java Testing") lives by the following rules of thumb:

- "Tests first" or "tests last" is unimportant as long as there are tests.
- Try to think about testing as early as possible in your development process.
- Don't listen to people who tell you to write "the simplest possible thing that could possibly work", also known as YAGNI. If your experience tells you you're going to need this extra class in the future even if it's not needed right now, follow your judgement and add it now.
- Keep in mind that functional tests are the only tests that really matter to your users. Unit tests are just a convenience for you, the developer. A luxury. If you have time to write unit tests, great: they will save you time down the road when you need to track bugs. But if you don't, make sure that your functional tests cover what your users expect from your product.
- Don't feel bad if you are not doing Test-Driven Development. There are a lot of factors that make this practice a bad fit for a lot of projects and developer personalities (aspects which are very often never mentioned by TDD extremists).

From [TDD leads to an architectural meltdown around iteration three](http://beust.com/weblog/archives/000477.html), by Cedric Beust.
