---
title: "Zend Framework DAL: DAOs and DataMappers"
date: "2009-09-19"
tags: [databases,design-patterns,frameworks,programming,software-architecture]
---

A Data Access Layer (DAL) is the layer of your application that provides simplified access to data stored in persistent storage of some kind. For example, the DAL might return a reference to an object complete with its attributes instead of a row of fields from a database table.

A Data Access Objects (DAO) is used to abstract and encapsulate all access to the data source. The DAO manages the connection with the data source to obtain and store data. Also, it implements the access mechanism required to work with the data source. The data source could be a persistent store like a database, a file or a Web service.

And finally, the DataMapper pattern is used to move data between the object and a database while keeping them independent of each other. The DataMapper main responsibility is to transfer data between the two and also to isolate them from each other.

Here's an example of the DataMapper pattern:

### Database Table Structure

```
CREATE TABLE `user` (
 `id` int(11) NOT NULL auto_increment,
 `first_name` varchar(100) NOT NULL,
 `last_name` varchar(100) NOT NULL,
 PRIMARY KEY  (`id`)
)
```

### The User DAO

The DAO pattern provides a simple, consistent API for data access that does not require knowledge of an ORM interface. DAO does not just apply to simple mappings of one object to one relational table, but also allows complex queries to be performed and allows for stored procedures and database views to be mapped into data structures.

A typical DAO design pattern interface is shown below:

```
interface UserDao
{
    public function fetchRow($id);
    public function fetchAll();
    public function insert($data);
    public function update($id, $data);
    public function delete($id);
}

class UserDatabaseDao implements UserDao
{
    public function fetchRow($id)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource();
        $db = $dataSource->getConnection('slave');
        $query = $db->select()->from('user')->where('id = ?', $id);
        return $db->fetchRow($query);
    }

    public function fetchAll()
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource();
        $db = $dataSource->getConnection('slave');
        $query = $db->select()->from('user');
        return $db->fetchAll($query);
    }

    public function insert($data)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource();
        $db = $dataSource->getConnection('master');
        $db->insert('user', $data);
        return $db->lastInsertId();
    }

    public function update($id, $data)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource();
        $db = $dataSource->getConnection('master');
        $condition = $db->quoteInto('id = ?', $id);
        return $db->update('user', $data, $condition);
    }

    public function delete($id)
    {
        $dataSource = Zf_Orm_Manager::getInstance()->getDataSource();
        $db = $dataSource->getConnection('master');
        $condition = $db->quoteInto('id = ?', $id);
        return $db->delete('user', $condition);
    }
}
```

### The User Entity

An Entity is anything that has continuity through a life cycle and distinctions independent of attributes that are important to the application's user.

```
class User
{
    protected $id;
    protected $firstName;
    protected $lastName;

    public function setId() {
    }
    public function getId() {
    }
    public function setFirstName() {
    }
    public function getFirstName() {
    }
    public function setLastName() {
    }
    public function getLastName() {
    }
    public function toArray() {
    }
}
```

### The User DataMapper

```
class UserDataMapper extends Zf_Orm_DataMapper
{
    public function __construct()
    {
        $this->setMap(
            array(
                'id'         => 'id',
                'first_name' => 'firstName',
                'last_name'  => 'lastName'
            )
        );
    }
}
```

Source Code: [http://fedecarg.com/repositories/show/datamapper](http://fedecarg.com/repositories/show/datamapper)

### The User Repository

Repositories play an important part in DDD, they speak the language of the domain and act as mediators between the domain and data mapping layers. They provide a common language to all team members by translating technical terminology into business terminology.

Lets create a UserRepository class to isolate the domain object from details of the DAO:

```
class UserRepository
{
    private $databaseDao;

    public funciton setDatabaseDao(UserDao $dao)
    {
        $this->databaseDao = $dao;
    }

    public function getDatabaseDao()
    {
        if (null === $this->databaseDao) {
            $this->setDatabaseDao(new UserDatabaseDao());
        }
        return $this->databaseDao;
    }

    public function find($id)
    {
        $row = $this->getDatabaseDao()->fetchRow($id);
        $mapper = new UserDataMapper();
        $user = $mapper->assign(new User(), $row);

        return $user;
    }
}
```

### The User Controller

```
class UserController extends Zend_Controller_Action
{
    public function viewAction()
    {
        $userRepository = new UserRepository();
        $user = $userRepository->find($this->_getParam('id'));
        if ($user instanceof User) {
            $id = $user->getId();
            $firstName = $user->getFirstName();
            $lastName = $user->getLastName();
            // get an array of key value pairs
            $row = $user->toArray();
        }
    }
}
```

That's all, I hope you've found this post useful.
