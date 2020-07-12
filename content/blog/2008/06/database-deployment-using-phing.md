---
title: "Agile Database Deployment Using Phing"
date: "2008-06-22"
tags: [databases,deployment,programming,software-architecture,tools,webdev]
---

Phing allows you to use SQL to define changes to your database schema, making it possible to use a version control system to keep things synchronized with the actual code.

A common way to automate development and deployment tasks is by writing shell scripts, however, Phing provides some advantages over shell scripts for task automation. Most of the shell scripts I created so far help me automate the most tedious tasks, from configuring, building, testing and documenting applications to synchronizing files and migrating database schemas. But, having a large collection of shell scripts can lead to a maintenance nightmare, reason why I decided to port some of them to PHP as Phing tasks. I chose Phing because it's simple, powerful and very easy to extend.

### DbDeployTask

Phing offers an optional task for making revisions to a database, it's called DbDeployTask. Dave Marshall wrote a [nice article about it](http://www.davedevelopment.co.uk/2008/04/14/how-to-simple-database-migrations-with-phing-and-dbdeploy/). After adding this task to my build script and testing it, I came to the conclusion that DbDeployTask is not very effective if you want to roll out incremental changes to the databases. Also, it introduces new problems to the database migration process.

For example, if you use DbDeployTask, the SQL script names must begin with a number that indicates the order in which they should be run:

```
basedir/
  database/
    deltas/
      1-create_foo_table.sql
      2-alter_foo_table.sql
      3-add_constraint_id_to_foo.sql
    history/
      up_20080622120000.sql
      down_20080622120000.sql
  public/
```

But, what happens when 2 or more developers are making changes to the same database design? How do they know which scripts have been executed? What if they don't use a central development database, how do they merge individual changes? To be honest, I don't know.

### Agile Database Deployment

A more Agile way to manage database changes is for the developers to connect to a central development database. After making any changes to the database, the developers will have to:

1. Update the local database/ directory before modifying any script.
2. Copy the changes made to the database to the corresponding SQL file (up and down).
3. Check the files back into the repository.

Finally, the changes can be propagated to other databases by executing a script or setting up a Cron job.

This is how the database directory can be structured:

```
basedir/
  database/
    down/
      alter/
      constraints/
      create/
      data/
    up/
      alter/
      constraints/
      create/
      data/
  public/
```

To put your database definition under version control, you need an off-line representation of that database. The directory up/ is used when migrating to a new version, down/ is used to roll back any changes if needed. These directories contain all the changes made to the database:

```
up/
  alter/
    foo.table.sql
  constraints/
    foo.table.sql
  create/
    foo.table.sql
  data/
```

This directory structure is based on the one [suggested by Mike Hostetler](http://amountaintop.com/how-i-build-application-part-2-database), and it clearly offers more advantages. After the team has made all necessary changes, the next step is to deploy the schema. If you are responsible for performing that task, you can get the most recent version of the database project from version control, build the deployment script, manually update that script as necessary, and then run the script to deploy the schema changes into development, staging or production.

### Learn More

[Database Schema Deployment (PDF)](http://pooteeweet.org/files/phptek06/database_schema_deployment.pdf) - By Lukas Smith
