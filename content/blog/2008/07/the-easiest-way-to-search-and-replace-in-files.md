---
title: "The easiest way to search and replace in files"
date: "2008-07-09"
tags: [linux,programming]
---

The "find" command is one of the most powerful and useful Unix commands, you can use it to locate files, and then perform some type of action on the files after they've been located. With this capability, you can locate files using powerful search criteria, and then run any Unix command you want on the files you locate.

### Find Strings

In the following example I'll recursively search a directory for files containing the 'test' string and output the name of the files and the line number:

```
$ grep -in -e 'test' `find -type f`
```

This command will search in the current directory and all sub directories. All files containing the string 'test' will have their path printed to standard output:

```
./build/build.properties:4:project.name=test
./build/build.xml:12:<target name="sync:test">
```

### Count Strings

If you only want to count the matching lines for each file, then you can use the following command:

```
$ grep -inc -e 'test' `find -type f` | grep -v :0
```

This will output:

```
./build/build.properties:1
./build/build.xml:2
```

### Replace Strings

And finally, if you want to replace all occurrences of the search string with the replacement string:

```
$ sed -i 's/search/replace/' `find -type f`
```

### Linux Commands

Learn more about these commands:

- [find](http://linux.about.com/od/commands/l/blcmdl1_find.htm)
- [grep](http://linux.about.com/od/commands/l/blcmdl1_grep.htm)
- [sed](http://linux.about.com/od/commands/l/blcmdl1_sed.htm)
