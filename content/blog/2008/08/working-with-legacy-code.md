---
title: "Working with legacy code"
date: "2008-08-20"
tags: [programming,software-architecture]
---

Most projects carry some amount of legacy code. You can't work very fast with a legacy code base, but you can speed it up if you establish a strategy to deal with your existing code and mitigate risk as new development goes forward.

### What is Legacy Code?

Legacy code is code from the past, maintained because it works. But, for people who deal with it day in and day out "legacy code" is a pandora's box: sleepless nights and anxious days poring through bad structure, code that works in some incomprehensible way, days adding features with no way of estimating how long it will take. The age of the code has nothing to do with it. People are writing legacy code right now, maybe on your project.

The key to working effectively with legacy code is getting it to a place where it is possible to know that you are making changes one at a time. When you can do that, you can nibble away at uncertainty incrementally.

### Legacy Management Strategy

By itself, legacy code doesn't hurt anything. As long as it works, it only becomes painful when you have to make modifications. Fortunately, the first steps you have to take to work effectively with legacy code also make it easier to clean things up.

1. Identify change points
2. Find an inflection point
3. Cover the inflection point
4. Break external dependencies
5. Break internal dependencies
6. Write tests
7. Make changes
8. Refactor the covered code

These steps are explained in more detail in the article [Working Effectively With Legacy Code (PDF)](http://www.objectmentor.com/resources/articles/WorkingEffectivelyWithLegacyCode.pdf), written by [Michael Feathers](http://www.amazon.com/Working-Effectively-Legacy-Robert-Martin/dp/0131177052).
