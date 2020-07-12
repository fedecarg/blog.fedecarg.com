---
title: "Domain-Driven Design: Sample Application"
date: "2009-03-22"
tags: [agile-development,design-patterns,frameworks,programming,software-architecture,webdev]
---

**Last updated: 15 Feb, 2010**

Part 1: [Domain-Driven Design and MVC Architectures](http://blog.fedecarg.com/2009/03/11/domain-driven-design-and-mvc-architectures/) Part 2: [Domain-Driven Design: Data Access Strategies](http://blog.fedecarg.com/2009/03/12/domain-driven-design-and-data-access-strategies/) Part 3: [Domain-Driven Design: The Repository](http://blog.fedecarg.com/2009/03/15/domain-driven-design-the-repository/)

Some of the Domain-driven design concepts explained above are applied in this sample application.

### Directory Structure

```
app/
    config/
    controllers/
        UserController.php
    domain/
        entities/
            User.php
            UserProfile.php
        repositories/
            UserRepository.php
    views/
lib/
public/
```

The domain layer should be well separated from the other layers and it should have few dependencies on the framework you are using.

### User Entity

The User and UserProfile objects have a one-to-one relationship and form an Aggregate. An Aggregate is as a collection of related objects that have references between each other. Within an Aggregate there’s always an Aggregate Root (parent Entity), in this case User:

```
class User
{
    private $id;
    private $name;

    /* @var UserProfile */
    private $profile;

    public function __construct($id, $name)
    {
        $this->id = $id;
        $this->name = $name;
    }

    public function setProfile(UserProfile $profile)
    {
        $this->profile = $profile;
    }

    public function getProfile()
    {
        return $this->profile;
    }
}

class UserProfile
{
    private $id;

    public function __construct($id)
    {
        $this->id = $id;
    }
}
```

### Users Collection

A collection is simply an object that groups multiple elements into a single unit.

```
class Users
{
    private $elements = array();

    public function __construct(array $users)
    {
        foreach ($users as $user) {
            if (!($user instanceof User)) {
                throw new Exception();
            }
            $this->elements[] = $user;
        }
    }

    public function toArray()
    {
        return $this->elements;
    }
}
```

### User DAO

The UserDAO class allows data access mechanisms to change independently of the code that uses the data:

```
interface UserDao
{
    public function find($id);
    public function findAll();
}

class UserDatabaseDao implements UserDao
{
    public function find($id)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource()
        $db = $dataSource->getConnection('slave');
        $query = $db->select()->from('user')->where('id = ?', $id);
        return $db->fetchRow($query);
    }

    public function findAll()
    {
        ...
        return $db->fetchAll($query);
    }
}

interface UserProfileDao
{
    public function find($id);
    public function findByUserId($id);
}

class UserProfileDatabaseDao implements UserProfileDao
{
    public function find($id)
    {
        ...
    }

    public function findByUserId($id)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource()
        $db = $dataSource->getConnection('slave');
        $query = $db->select()->from('user_profile')->where('user_id = ?', $id);
        return $db->fetchRow($query);
    }
}
```

### User Repository

A Repository is basically a collection of Aggregate Roots. Collections are used to store, retrieve and manipulate Entities and Value objects, however, object management is beyond the scope of this post. The Repository object can inject dependencies on demand, making the instantiation process inexpensive.

```
class UserRepository
{
    /* @var UserDatabaseDao */
    private $userDao;

    /* @var UserProfileDatabaseDao */
    private $userProfileDao;

    public function __construct()
    {
    	$this->userDao = new UserDatabaseDao();
    	$this->userProfileDao = new UserProfileDatabaseDao();
    }

    public function find($id)
    {
        $row = $this->userDao->find($id);
        $user = new User($row['id'], $row['name']);

        $row = $this->userProfileDao->findByUserId($id);
        if (isset($row['id'])) {
            $profile = new UserProfile($row['id']);
            $user->setProfile($profile);
        }

        return $user;
    }

    public function findAll()
    {
        $users = array();
        $rows = $this->userDao->findAll();
        foreach ($rows as $row) {
            $users[] = new User($row['id'], $row['name']);
        }
        return new Users($users);
    }
}
```

**Usage:**

```
$repository = new UserRepository();
$user = $repository->find(1);
$profile = $user->getProfile();
$users = $repository->findAll();
```

### Source Code

[http://svn.fedecarg.com/repo/Zf/Orm](http://svn.fedecarg.com/repo/Zf/Orm)

### Links

If you're interested in learning more about Domain-driven design, I recommend the following articles:

[Domain Driven Design and Development In Practice](http://www.infoq.com/articles/ddd-in-practice) [](http://www.infoq.com/articles/ddd-evolving-architecture)[Domain-Driven Design in an Evolving Architecture](http://www.infoq.com/articles/ddd-evolving-architecture)
