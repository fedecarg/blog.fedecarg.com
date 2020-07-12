---
title: "Domain-Driven Design: The Repository"
date: "2009-03-15"
tags: [agile-development,design-patterns,frameworks,programming,software-architecture,webdev]
---

Part 2: [Domain-Driven Design: Data Access Strategies](http://blog.fedecarg.com/2009/03/12/domain-driven-design-and-data-access-strategies/)

### The Ubiquitous Language

The ubiquitous language is the foundation of Domain-driven design. The concept is simple, developers and domain experts share a common language that both understand. This language is set in business terminology, not technical terminology. This ubiquitous language allows the technical team become part of the business.

### The Repository

Repositories play an important part in DDD, they speak the language of the domain and act as mediators between the domain and data mapping layers. They provide a common language to all team members by translating technical terminology into business terminology.

In a nutshell, a Repository:

- Is not a data access layer
- Provides a higher level of data manipulation
- Is persistence ignorance
- Is a collection of aggregate roots
- Offers a mechanism to manage entities

### Data Access Objects

In the absence of an ORM framework, the Data Access Object (DAO) handles the impedance mismatch that a relational database has with object-oriented techniques. In DDD, you inject Repositories, not DAO's in domain entities. For example, imagine you have an entity named User that needs to access the database to retrieve the User details:

```
class User
{
    private $id;
    private $name;

    public function __construct(UserDao $dao, $id)
    {
        $row = $dao->find($id);
        $this->setId($row['id']);
        $this->setName($row['name']);
    }

    public function setId($id) {}
    public function getId() {}
    public function setName($name) {}
    public function getName() {}
}
```

The User DAO class will look something like this:

```
interface UserDao
{
    public function fetchRow($id);
}

class UserDatabaseDaoImpl implements UserDao
{
    public function fetchRow($id)
    {
        ...
        $query = $db->select();
        $query->from('user');
        $query->where('id = ?', $id);
        return $db->fetchRow($query);
    }
}

$dao = new UserDatabaseDaoImpl();
$user = new User($dao, 1);
$userId = $user->getId();
```

But, what about separation of concerns? DAO's are related to persistence, and persistence is infrastructure, not domain. The main problem with the example above is that we have lots of different concerns polluting the domain. According to DDD, an object should be distilled until nothing remains that does not relate to its meaning or support its role in interactions. And that's exactly the problem the Repository pattern tries to solve.

### Injecting Repositories

Lets create a UserRepository class to isolate the domain object from details of the UserDatabaseDaoImpl class:

```
class User
{
    private $id;
    private $name;

    public function __construct(UserRepository $repository, $id)
    {
        $row = $repository->find($id);
        $this->setId($row['id']);
        $this->setName($row['name']);
    }

    public function setId($id) {}
    public function getId() {}
    public function setName($name) {}
    public function getName() {}
}
```

It's the responsibility of the UserRepository to work with all necessary DAO's and provide all data access services to the domain model in the language which the domain understands.

```
interface UserRepository
{
    public function find($id);
}

class UserRepositoryImpl implements UserRepository
{
    private $databaseDao;

    public function setDatabaseDao(UserDao $dao) {}
    public function getDatabaseDao() {}

    public function find($id)
    {
        return $this->getDatabaseDao()->find($id);

    }
}

$userRepository = new UserRepositoryImpl();
$userRepository->setDatabaseDao(new UserDatabaseDaoImpl());

$user = new User($userRepository, 1);
$userId = $user->getId();
$userName = $user->getName();
```

The main difference between the Repository and the DAO is that the DAO is at a lower level of abstraction and doesn't speak the ubiquitous language of the domain.

### So, what's next?

The process of creating an entity is complex, because an entity always has relationship with other objects in your domain model. When creating an entity, we have to initialize its relationships as well. Therefore, it's a good practice to delegate this task to another object.

We've seen how to write a persistence-ignorant domain model. Next, I'll explain how to automate the creation and injection of dependencies.

Part 4: [Domain-Driven Design: Sample Application](http://blog.fedecarg.com/2009/03/22/domain-driven-design-sample-application/)
