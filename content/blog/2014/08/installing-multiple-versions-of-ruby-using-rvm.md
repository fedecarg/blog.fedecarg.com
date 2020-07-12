---
title: "Installing multiple versions of Ruby using RVM"
date: "2014-08-26"
tags: [programming]
---

Ruby Version Manager (RVM) is a tool that allows you to install multiple versions of Ruby and have multiple versions of the same interpreter. Very handy for those who have to maintain different applications using different versions of Ruby.

To start, download RVM and install the latest stable version of Ruby:

```
$ echo insecure >> ~/.curlrc
$ curl -L https://get.rvm.io | bash -s stable --ruby
$ source ~/.bash_profile
```

Install an old version of Ruby:

```
$ rvm install 1.8.6
$ rvm use 1.8.6 --default
$ ruby -v
ruby 1.8.6
```

Create a Gem set and install an old version of Rails:

```
$ rvm gemset create rails123
$ gem install rails -v 1.2.3
$ rails -v
Rails 1.2.3
```

Switch back to your system:

```
$ rvm system
$ rails -v
Rails 2.3.5
```

Switch back to your RVM environment:

```
$ rvm 1.8.6@rails123
```

And, if you want to remove Rails 1.2.3, just delete the Gem set:

```
$ rvm gemset delete rails123
```

Alternatively to RVM, you also might look into [rbenv](https://github.com/sstephenson/rbenv).
