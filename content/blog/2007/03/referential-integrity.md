---
title: "Referential integrity"
date: "2007-03-30"
tags: [webdev]
---

Recent versions of MySQL have implemented support for foreign keys through the new InnoDB table engine. We explain how it works

Referential integrity is an important concept in database design. The term refers to a state when all the references in a database are valid and no invalid links exist between the various tables that make up the system. When referential integrity exists, any attempt to link to a record which does not already exist will fail; this helps prevent user errors, producing a more accurate (and useful) database.

Referential integrity is usually implemented through the use of foreign keys. For a long time, the popular open-source RDBMS MySQL did not support foreign keys, citing concerns that such support would erode RDBMS speed and performance. However, given the high volume of user interest in this feature, later versions of MySQL implemented support for foreign keys through the InnoDB table engine. Consequently, maintaining referential integrity within the tables that make up a database is significantly simpler.

In order to set up a foreign key relationship between two MySQL tables, three conditions must be met:

1. Both tables must be of the InnoDB table type.
2. The fields used in the foreign key relationship must be indexed.
3. The fields used in the foreign key relationship must be similar in data type.

The best way to understand how this works is with an example. Begin by creating two tables, one listing animal species and their corresponding codes (table name: species) and the other listing animals in a zoo (table name: zoo). The idea here is to link the two tables by the species code, so that only those entries in the zoo table which have a valid species code in the species table are accepted and saved to the database.

```

CREATE TABLE species (id TINYINT NOT NULL AUTO_INCREMENT,
                      name VARCHAR(50) NOT NULL,
                      PRIMARY KEY(id)) ENGINE=INNODB;

INSERT INTO species VALUES (1, 'orangutan'), (2, 'elephant'),
                           (3, 'hippopotamus'), (4, 'yak');

CREATE TABLE zoo (id INT(4) NOT NULL,
                  name VARCHAR(50) NOT NULL,
                  FK_species TINYINT(4) NOT NULL,
                  INDEX (FK_species),
                  FOREIGN KEY (FK_species) REFERENCES species (id),
                  PRIMARY KEY(id)) ENGINE=INNODB;
```

Important: For non-InnoDB tables, the

```
FOREIGN KEY
```

clause is ignored.

As the above illustrates, a foreign key relationship now exists between the

```
fieldszoo.species
```

and

```
species.id
```

. An entry in the

```
zoo
```

table will be permitted only if the corresponding

```
zoo.species
```

field matches a value in the

```
species.idfield
```

. This is clearly visible in the following output, which demonstrates what happens when you attempt to enter a record for Harry Hippopotamus with an invalid species code:

```
mysql> INSERT INTO zoo VALUES (1, 'Harry', 5);
ERROR 1216 (23000): Cannot add or update a child row: a foreign key
constraint fails
```

Here, MySQL checks the speciestable to see if the species code exists and, finding that it does not, rejects the record. Contrast this with what happens when you enter the same record with a valid species code (one that already exists in the species table):

```
mysql> INSERT INTO zoo VALUES (1, 'Harry', 3);
Query OK, 1 row affected (0.06 sec)
```

Here, MySQL checks the species table to see if the species code exists and, finding that it does, permits the record to be saved to the zoo table.

To delete a foreign key relationship, first use the

```
SHOW CREATE TABLE
```

command to find out InnoDB's internal label for the field.

```

CREATE TABLE `zoo` (`id` int(4) NOT NULL default '0',
                    `name` varchar(50) NOT NULL default '',
                    `FK_species` tinyint(4) NOT NULL default '0',
                    KEY `FK_species` (`FK_species`),
                    CONSTRAINT `zoo_ibfk_1` FOREIGN KEY (`FK_species`)
                    REFERENCES `species` (`id`))
                    ENGINE=InnoDB DEFAULT CHARSET=latin1
```

And then use the

```
ALTER TABLE
```

command with the

```
DROP FOREIGN KEY
```

clause, as below:

```
mysql> ALTER TABLE zoo DROP FOREIGN KEY zoo_ibfk_1;
Query OK, 1 row affected (0.11 sec)
Records: 1  Duplicates: 0  Warnings: 0
```

To add a foreign key to an existing table, use the

```
ALTER TABLE
```

command with an

```
ADD FOREIGN KEY
```

clause to define the appropriate field as a foreign key:

```
mysql> ALTER TABLE zoo ADD FOREIGN KEY (FK_species) REFERENCES species (id);
Query OK, 1 rows affected (0.11 sec)
Records: 1  Duplicates: 0  Warnings: 0
```

As the examples above illustrate, foreign key relationships can play an important role in catching data entry errors, and implementing them usually results in a stronger, better-integrated database. On the other hand, it's worthwhile noting that performing foreign key checks is a resource-intensive process and defining complicated inter-relationships between tables can result in a significant performance drop. Therefore, it's important to always balance referential integrity considerations with performance considerations, and use foreign keys judiciously to ensure an optimal mix of speed and strength.
