---
title: "MySQL Split String Function"
date: "2009-02-22"
tags: [databases]
---

MySQL does not include a function to split a delimited string. However, it's very easy to create your own function.

### Create function syntax

A [user-defined function](http://dev.mysql.com/doc/refman/5.0/en/create-procedure.html) is a way to extend MySQL with a new function that works like a native MySQL function.

```
CREATE [AGGREGATE] FUNCTION function_name
RETURNS {STRING|INTEGER|REAL|DECIMAL}
```

To create a function, you must have the INSERT privilege for the <mysql> database.

### Split delimited strings

The following example function takes 3 parameters, performs an operation using an SQL function, and returns the result.

**Function**

```
CREATE FUNCTION SPLIT_STR(
  x VARCHAR(255),
  delim VARCHAR(12),
  pos INT
)
RETURNS VARCHAR(255)
RETURN REPLACE(SUBSTRING(SUBSTRING_INDEX(x, delim, pos),
       LENGTH(SUBSTRING_INDEX(x, delim, pos -1)) + 1),
       delim, '');
```

**Usage**

```
SELECT SPLIT_STR(string, delimiter, position)
```

**Example**

```
SELECT SPLIT_STR('a|bb|ccc|dd', '|', 3) as third;

+-------+
| third |
+-------+
| ccc   |
+-------+
```
