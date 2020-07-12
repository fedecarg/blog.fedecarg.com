---
title: "Cloning your Ubuntu Installation"
date: "2008-10-22"
tags: [linux]
---

It's always handy to have a complete list of packages installed, specially if you want to create a system that is similar to a different system you have already set up. In this post I'll cover how you can export a list of installed packages on one Ubuntu system, and import them into another to build a duplicate system.

### Make a copy of the system's repositories

Copy the /etc/apt/sources.list text file to the destination system over the network.

### Export the list of installed packages

```
$ dpkg --get-selections | grep '[[:space:]]install$' | \\
awk '{print $1}' > ~/package.list
```

Copy the package.list text file to the destination system over the network.

### Prepare the destination system

Update your package list to make sure you get the latest version of the packages:

```
$ apt-get update
```

### Import the package list

```
$ cat ~/package.list | xargs apt-get install
```

That's it! All of the files from the package list will have been imported into the new system. This doesn't mean that all of the settings have transferred over. To do that, you will likely need to copy settings from the /etc directory.
