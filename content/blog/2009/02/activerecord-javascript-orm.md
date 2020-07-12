---
title: "ActiveRecord: JavaScript ORM Library"
date: "2009-02-16"
tags: [databases,javascript,open-source,programming]
---

[Aptana](http://www.aptana.com/) has just released a beta version of its ActiveRecord.js which is an ORM JavaScript library that implements the ActiveRecord pattern. It works with AIR and other environments:

ActiveRecord.js is a single file, MIT licensed, relies on no external JavaScript libraries, supports automatic table creation, data validation, data synchronization, relationships between models, life cycle callbacks and can use an in memory hash table to store objects if no SQL database is available.

Example:

```
var User = ActiveRecord.define('users',{
    username: '',
    email: ''
});
User.hasMany('articles');

var ryan = User.create({
    username: 'ryan',
    email: 'rjohnson@aptana.com'
});

var Article = ActiveRecord.define('articles',{
    name: '',
    body: '',
    user_id: 0
});
Article.belongsTo('user');

var a = Article.create({
    name: 'Announcing ActiveRecord.js',
    user_id: ryan.id
});
a.set('name','Announcing ActiveRecord.js!!!');
a.save();

a.getUser() == ryan;
ryan.getArticleList()[0] == a;
```

### Links

- [Project Website](http://www.activerecordjs.org/)
- [Aptana: ActiveRecord.js Released as Beta](http://www.aptana.com/node/547)
