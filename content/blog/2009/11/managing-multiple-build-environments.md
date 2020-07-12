---
title: "Managing Multiple Build Environments"
date: "2009-11-14"
tags: [agile-development,deployment,java,linux,tools,web-apps]
---

**Last updated: 3 March, 2010**

One of the challenges of Web development is managing multiple build environments. Most applications pass through several environments before they are released. These environments include: A local development environment, a shared development environment, a system integration environment, a user acceptance environment and a production environment.

### Automated Builds

Automated builds provide a consistent method for building applications and are used to give other developers feedback about whether the code was successfully integrated or not. There are different types of builds: Continuous builds, Integration builds, Release builds and Patch builds.

A source control system is the main point of integration for source code. When your team works on separate parts of the code base, you have to ensure that your checked in code doesn't break the Integration build. That's why it is important that you run your unit tests locally before checking in code.

Here is a recommended process for checking code into source control:

- Get the latest code from source control before running your tests
- Verify that your local build is building and passing all the unit tests before checking in code
- Use hooks to run a build after a transaction has been committed
- If the Integration build fails, fix the issue because you are now blocking other developers from integrating their code

Hudson can help you automate these tasks. It's extremely [easy to install](http://www.davegardner.me.uk/blog/2009/11/09/continuous-integration-for-php-using-hudson-and-phing/) and can be configured entirely from a Web UI. Also, it can be extended via plug-ins and can execute Phing, Ant, Gant, NAnt and Maven build scripts.

### Build File

We need to create a master build file that contains the actions we want to perform. This script should make it possible to build the entire project with a single command line.

First we need to separate the source from the generated files, so our source files will be in the "src" directory and all the generated files in the "build" directory. By default Ant uses build.xml as the name for a build file, this file is usually located in the project root directory.

Then, you have to define whatever environments you want:

```
project/
    build/
        files/
            local/
            development/
            integration/
            production/
        packages/
            development/
                project-development-0.1-RC.noarch.rpm
            integration/
            production/
        default.properties
        local.properties
        development.properties
        production.properties
    src/
        application/
            config/
            controllers/
            domain/
            services/
            views/
        library/
        public/
    tests/
    build.xml
```

Build files tend to contain the same actions:

- Delete the previous build directory
- Copy files
- Manage dependencies
- Run unit tests
- Generate HTML and XML reports
- Package files

The target element is used as a wrapper for a sequences of actions. A target has a name, so that it can be referenced from elsewhere, either externally from the command line or internally via the "depends" or "antcall" keyword. Here's a basic build.xml example:

```
<?xml version="1.0" encoding="iso-8859-1"?>
<project name="project" basedir="." default="main">

    <target name="init"></target>
    <target name="test"></target>
    <target name="test-selenium"></target>
    <target name="profile"></target>
    <target name="clean"></target>
    <target name="build" depends="init, test, profile, clean"></target>
    <target name="package"></target>

</project>
```

The property element allows the declaration of properties which are like user-definable variables available for use within an Ant build file. Properties can be defined either inside the buildfile or in a standalone properties file. For example:

```
<?xml version="1.0" encoding="iso-8859-1"?>
<project name="project" basedir="." default="main">

    <property file="${basedir}/build/default.properties" />
    <property file="${basedir}/build/${build.env}.properties" />
    ...

</project>
```

The core idea is using property files which name accords to the environment name. Then simply use the custom build-in property build.env. For better use you should also provide a file with default values. The following example intends to describe a typical Ant build file, of course, it can be easily modified to suit your personal needs.

```
<?xml version="1.0" encoding="iso-8859-1"?>
<project name="project" basedir="." default="main">

    <property file="${basedir}/build/default.properties" />
    <condition property="build.env" value="${build.env}" else="local">
        <isset property="build.env" />
    </condition>
    <property file="${basedir}/build/${build.env}.properties" />

     <property environment="env" />
     <condition property="env.BUILD_ID" value="${env.BUILD_ID}" else="">
         <isset property="env.BUILD_ID" />
     </condition>

    <target name="init">
        <echo message="Environment: ${build.env}"/>
        <echo message="Hudson build ID: ${env.BUILD_ID}"/>
        <echo message="Hudson build number: ${env.BUILD_NUMBER}"/>
        <echo message="SVN revision: ${env.SVN_REVISION}"/>
        <tstamp>
            <format property="build.datetime" pattern="dd-MMM-yy HH:mm:ss"/>
        </tstamp>
        <echo message="Build started at ${build.datetime}"/>
    </target>

    <target name="test">
        ...
    </target>

    <target name="clean">
        <delete dir="${build.dir}/files/${build.env}"/>
        <delete dir="${build.dir}/packages/${build.env}"/>
        <mkdir dir="${build.dir}/files/${build.env}"/>
        <mkdir dir="${build.dir}/packages/${build.env}"/>
    </target>

    <target name="build" depends="init, test, profile, clean">
        ...
    </target>
    ...

</project>
```

Using ant -Dname=value lets you define values for properties on the Ant command line. These properties can then be used within your build file as any normal property: ${name} will put in value.

```
$ ant build -Dbuild.env=development
```

There are different ways to target multiple environments. I hope I have covered enough of the basic functionality to get you started.
