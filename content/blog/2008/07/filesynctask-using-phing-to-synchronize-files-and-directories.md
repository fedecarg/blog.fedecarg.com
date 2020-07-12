---
title: "FileSyncTask: Using Phing to synchronize files and directories"
date: "2008-07-21"
tags: [deployment,linux,programming,webdev]
---

I needed to automate the task of synchronizing files from one server to another, so I wrote a Phing task. Finally today I found some time to finish writing the documentation.

### Overview

FileSyncTask is a Phing extension for Unix systems which synchronizes files and directories from one location to another while minimizing data transfer. FileSyncTask can copy or display directory contents and copy files, optionally using compression and recursion.

Rather than using FTP or some other form of file transfer, FileSyncTask uses rsync to copy only the diffs of files that have actually changed. Only actual changed pieces of files are transferred, rather than the whole file, which results in transferring only a small amount of data and are very fast. FTP, for example, would transfer the entire file, even if only one byte changed. The tiny pieces of diffs are then compressed on the fly, further saving you file transfer time and reducing the load on the network.

FileSyncTask can be used to synchronize Website trees from staging to production servers and to backup key areas of the filesystems.

### Usage

There are 4 different ways of using FileSyncTask:

1. For copying local files.
2. For copying from the local machine to a remote machine using a remote shell program as the transport (ssh).
3. For copying from a remote machine to the local machine using a remote shell program.
4. For listing files on a remote machine.

The SSH client called by FileSyncTask uses settings from the build.properties file:

```
sync.source.projectdir=/home/development.com/public
sync.destination.projectdir=/home/staging.com
sync.remote.host=server.com
sync.remote.user=user
sync.destination.backupdir=/home/staging.com/backup
sync.exclude.file=/home/development.com/build/sync.exclude
```

**Listing files**

The "listonly" option will cause the modified files to be listed instead of transferred. You must specify a source and a destination, one of which may be remote.

```

lt;taskdef name="sync" classname="phing.tasks.ext.FileSyncTask" />
<sync
    sourcedir="${sync.source.projectdir}"
    destinationdir="${sync.destination.projectdir}"
    listonly="true" verbose="true" />
```

**Excluding irrelevant files**

To exclude files from synchronizations, open and edit the sync.exclude file under the build/ directory. Each line can contain a file, a directory, or a pattern:

```
*~
.svn
.htaccess
public/index.development.php
public/images/uploads/*
build/*
log/*
tmp/*
```

**Copying files to a remote machine**

The following task definition will transfer files from a local source to a remote destination:

```

<taskdef name="sync" classname="phing.tasks.ext.FileSyncTask" />
<sync
    sourcedir="${sync.source.projectdir}"
    destinationdir="${sync.remote.user}@${sync.remote.host}:${sync.destination.projectdir}"
    excludefile="${sync.exclude.file}"
    verbose="true" />
```

### Example

**Directory structure**

In order to separate the sync settings from the main build file, I've created a file called sync.properties:

```
development.com
|-- build
|   |-- build.properties
|   |-- build.xml
|   |-- sync.exclude
|   `-- sync.properties
`-- public
    `-- index.php
```

**XML build file**

Phing uses XML build files that contain a description of the things to do. The build file is structured into targets that contain the actual commands to perform:

```
<?xml version="1.0" ?>
<project name="example" basedir="." default="build">
<property name="version" value="1.0" />

<!-- Public targets -->
<target name="sync:list" description="List files">
  <phingcall target="-sync-execute-task">
    <property name="listonly" value="true" />
  </phingcall>
</target>

<target name="sync" description="Copy files">
  <phingcall target="-sync-execute-task">
    <property name="listonly" value="false" />
  </phingcall>
</target>

<!-- Private targets -->
<target name="-init" description="Load main settings">
  <tstamp />
  <property file="build.properties" />
</target>

<target name="-sync-execute-task" depends="-init">
  <property file="sync.properties" />
  <if>
    <not>
      <isset property="sync.verbose" />
    </not>
    <then>
      <property name="sync.verbose" value="true" override="true" />
      <echo message="The value of sync.verbose has been set to true" />
    </then>
  </if>
  <property name="sync.remote.auth" value="${sync.remote.user}@${sync.remote.host}" />
  <taskdef name="sync" classname="phing.tasks.ext.FileSyncTask" />
  <sync
    sourcedir="${sync.source.projectdir}"
    destinationdir="${sync.remote.auth}:${sync.destination.projectdir}"
    backupdir="${sync.remote.auth}:${sync.destination.backupdir}"
    excludefile="${sync.exclude.file}"
    listonly="${listonly}"
    verbose="${sync.verbose}" />
</target>
</project>
```

**Execute task**

```
$ phing sync:list
```

Outputs:

```
Buildfile: /home/development.com/build/build.xml

example > sync:list:
[phingcall] Calling Buildfile '/home/development.com/build/build.xml'
            with target '-sync-execute-task'

example > -init:
[property] Loading /home/development.com/build/build.properties

example > -sync-execute-task:
[property] Loading /home/development.com/build/sync.properties
[echo] The value of sync.verbose has been set to true

Execute Command
----------------------------------------
rsync -razv --list-only -b --backup-dir

Sync files to remote server
----------------------------------------
Source:        /home/development.com/public
Destination:   user@server.com:/home/staging.com
Backup:        user@server.com:/home/staging.com/backup

Exclude patterns
----------------------------------------
*~
.svn
.htaccess
public/index.development.php
public/images/uploads/*
build/*
log/*
tmp/*

(list of files that have changed)

BUILD FINISHED
Total time: 1.9763 second
```

### More information

- [FileSyncTask Documentation](http://www.fedecarg.com/wiki/FileSyncTask)
- [Source Code](http://svn.fedecarg.com/repo/Phing/tasks/ext/)

### Related Articles

- [Phing Documentation](http://phing.info/docs/guide/current/)
- [Rolling your own Phing task](http://raphaelstolt.blogspot.com/2007/03/rolling-your-own-phing-task.html)
- [Six valuable Phing build file refactorings](http://raphaelstolt.blogspot.com/2008/07/six-valuable-phing-build-file.html)
