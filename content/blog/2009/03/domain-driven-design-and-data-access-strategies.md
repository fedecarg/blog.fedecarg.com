---
title: "Domain-Driven Design: Data Access Strategies"
date: "2009-03-12"
tags: [agile-development,design-patterns,frameworks,programming,software-architecture]
---

Part 1: [Domain-Driven Design and MVC Architectures](http://blog.fedecarg.com/2009/03/11/domain-driven-design-and-mvc-architectures/)

### The Domain Model

Here are some of the features a Domain-driven design framework should support:

- A domain model that is independent and decoupled from the application.
- A reusable library that can be used in many different domain-specific applications.
- Dependency Injection in order to inject Repositories and Services into Domain Objects.
- Integration with unit testing frameworks, such as PHPUnit.
- Good integration with other frameworks, such as Zend, Symfony, Doctrine, etc.

The Zend Framework, for example, is part of the infrastructure layer and acts as a supporting library for all the other layers. However, the domain layer should be well isolated from the other layers of the application and should not be dependent on the framework you are using.

### Data Access Strategies

When accessing data from a data source, you have to decide how your application will communicate with a data source. Each data access strategy has its own advantages and disadvantages. Here are some design patterns that support DDD:

**Generic DAO's**

Data Access Objects (DAO's) and Repositories play an important role in DDD. The goal of a DAO is to abstract and encapsulate all access to the data and provide an interface. The DAO always connects, reads and saves data to a data source. From the applications point of view, it makes no difference when it accesses a database, XML file or Web service:

```
$user = new UserDbDao();
$row = $user->findUserById(456);
echo $row->name;

$user = new UserXmlDao();
$row = $user->findUserById(456);
echo $row->name;
```

**Table Data Gateway**

An object that acts as a Gateway to a database table. One instance handles all the rows in the table. The [Zend\_Db\_Table](http://framework.zend.com/manual/en/zend.db.table.html) solution is an implementation of the Table Data Gateway pattern:

```
$user = new User();
$rows = $user->find(456);
$row = $rows->current();
echo $row->name;
```

More info: [Table Data Gateway](http://martinfowler.com/eaaCatalog/tableDataGateway.html)

**Row Data Gateway**

An object that acts as a Gateway to a single record in a data source. There is one instance per row. [Zend\_Db\_Table\_Row](http://framework.zend.com/manual/en/zend.db.table.row.html) is an implementation of the Row Data Gateway pattern:

```
$user = new User();
$row = $user->fetchRow($user->select()->where('user_id = ?', 1));
echo $row->name;
```

More info: [Row Data Gateway](http://martinfowler.com/eaaCatalog/rowDataGateway.html)

**Active Record**

An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data. The [Mad\_Model component](http://framework.maintainable.com/mvc/3_model.php) is an  ORM layer that follows the Active Record pattern where tables map to classes, rows map to objects, and columns to object attributes:

```
$user = new User();
$userFinder = new UserFinder();
$row = $userFinder->find(456);
```

More info: [Active Record](http://martinfowler.com/eaaCatalog/activeRecord.html)

**Data Mapper**

A layer of Mappers that moves data between objects and a database while keeping them independent of each other and the mapper itself:

```
$user = new User();
$userMapper = new UserMapper();
$row = $userMapper->findByEmail($email);
```

More info: [Data Mapper](http://martinfowler.com/eaaCatalog/dataMapper.html)

**Data Transfer Objects**

A Data Transfer Object (DTO) in Domain-driven design is not the same as a Value Object. A DTO is often used in combination with a DAO to encapsulate data. It allows you to reduce communication effort when dealing with a lot of small data entities. For example:

```
class UserData
{
    public function setId($id) {}
    public function getId() {}
    public function setFirstName($firstName) {}
    public function getFirstName() {}
    public function setLastName($lastName) {}
    public function getLastName() {}
}

$user = new UserData();
$user->setFirstName('Jim');
$user->setLastName('Morrison');

$userDao = new UserDbDao();
$userDao->insert($user);
```

A DTO is a simple container for a set of aggregated data. It should contain no business logic and limit its behaviour to activities such as internal consistency checking and basic validation. Be careful not to make the DTO depend on any new classes as a result of implementing these methods.

Part 3: [Domain-Driven Design: The Repository](http://blog.fedecarg.com/2009/03/15/domain-driven-design-the-repository/)
