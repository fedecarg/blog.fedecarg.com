---
title: "Implementing Dynamic Finders and Parsing Method Expressions"
date: "2010-03-22"
tags: [databases,frameworks,open-source,programming,software-architecture,webdev]
---

Most ORMs support the concept of dynamic finders. A dynamic finder looks like a normal method invocation, but the method itself doesn't exist, instead, it's generated dynamically and processed via another method at runtime.

A good example of this is Ruby. When you invoke a method that doesn't exist, it raises a NoMethodError exception, unless you define "method\_missing". Rails [ActiveRecord::Base](http://dev.rubyonrails.org/svn/rails/tags/rel_2-0-2/activerecord/lib/active_record/base.rb) class implements some of its magic thanks to this method. For example, find\_by\_title(title) and find\_by\_title\_and\_date(title, date) are turned into:

```
find(:first, :conditions => ["title = ?", title])
find(:first, :conditions => ["title = ? AND date = ?", title, date])
```

What's nice about Ruby is that the language allows you to [define methods dynamically](http://blog.jayfields.com/2008/02/ruby-dynamically-define-method.html) using the "define\_method" method. That's how Rails defines each dynamic finder in the class after it is first invoked, so that future attempts to use it do not run through the "method\_missing" method.

### **Method Expressions**

GORM, Grails ORM library, introduces the concept of dynamic method expressions. A method expression is made up of the prefix such as "findBy" followed by an expression that combines one or more properties. Grails takes advantage of Groovy features to provide dynamic methods:

```
findByTitle("Example")
findByTitleLike("Exa%")
```

Method expressions can also use a boolean operator to combine two criteria:

```
findAllByTitleLikeAndDateGreaterThan("Exampl%", '2010-03-23')
```

In this case we are using AND in the middle of the query to make sure both conditions are satisfied, but you could equally use OR:

```
findAllByTitleLikeOrDateGreaterThan("Exampl%", '2010-03-23')
```

### **Parsing Method Expressions**

[MethodExpressionParser](http://fedecarg.com/wiki/expressionparser/Home) is a PHP library for parsing method expressions. It's designed to quickly and easily parse method expressions and construct conditions based on attribute names and arguments.

**Description**

```
[finderMethod]([attribute][expression][logicalOperator])?[attribute][expression]
```

**Expressions**

- LessThan: Less than the given value
- LessThanEquals: Less than or equal a give value
- GreaterThan: Greater than a given value
- GreaterThanEquals: Greater than or equal a given value
- Like: Equivalent to a SQL like expression
- NotEqual: Negates equality
- IsNotNull: Not a null value (doesn't require an argument)
- IsNull: Is a null value (doesn't require an argument)

**Examples**

```
findByTitleAndDate('Example', date('Y-m-d'));
SELECT * FROM book WHERE title = ? AND date = ?

findByTitleOrDate('Example', date('Y-m-d'))
SELECT * FROM book WHERE title = ? OR date = ?

findByPublisherOrTitleAndDate('Name', 'Example', date('Y-m-d'))
SELECT * FROM book WHERE publisher = ? OR (title = ? AND date = ?)

findByPublisherInAndTitle(array('Name1', 'Name2'), 'Example')
SELECT * FROM book WHERE publisher IN (?, ?) AND date = ?

findByTitleLikeAndDateNotNull('Examp%')
SELECT * FROM book WHERE title LIKE ? AND date NOT NULL

findByIdOrTitleAndDateNotNull(1, 'Example')
SELECT * FROM book WHERE (id = ?) OR (title = ? AND date NOT NULL)
```

**Example 1:**

```
findByTitleLikeAndDateNotNull('Examp%');
```

Outputs:

```
array
  0 =>
    array
      0 =>
        array
          'attribute' => string 'title'
          'expression' => string 'Like'
          'format' => string '%s LIKE ?'
          'placeholders' => int 1
          'argument' => string 'Examp%'
      1 =>
        array
          'attribute' => string 'date'
          'expression' => string 'NotNull'
          'format' => string '%s IS NOT NULL'
          'placeholders' => int 0
          'argument' => null
```

**Example 2:**

```
findByTitleAndPublisherNameOrTitleAndPublisherName('Title', 'a', 'Title', 'b');
```

Outputs:

```
array
  0 =>
    array
      0 =>
        array
          'attribute' => string 'title'
          'expression' => string 'Equals'
          'format' => string '%s = ?'
          'placeholders' => int 1
          'argument' => string 'Title'
      1 =>
        array
          'attribute' => string 'publisher_name'
          'expression' => string 'Equals'
          'format' => string '%s = ?'
          'placeholders' => int 1
          'argument' => string 'a'
  1 =>
    array
      0 =>
        array
          'attribute' => string 'title'
          'expression' => string 'Equals'
          'format' => string '%s = ?'
          'placeholders' => int 1
          'argument' => string 'Title'
      1 =>
        array
          'attribute' => string 'publisher_name'
          'expression' => string 'Equals'
          'format' => string '%s = ?'
          'placeholders' => int 1
          'argument' => string 'b'
```

See more examples: [Project Wiki](http://fedecarg.com/wiki/expressionparser/Home)

### **Usage**

```
class EntityRepository
{
    private $methodExpressionParser;

    // Return a single instance of MethodExpressionParser
    public function getMethodExpressionParser() {
    }

    // Finder methods
    public function findBy($conditions) {
        var_dump($conditions);
    }
    public function findAllBy($conditions) {
        var_dump($conditions);
    }

    // Invoke finder methods
    public function __call($method, $args) {
        if ('f' === $method{0}) {
            try {
                $result = $this->getMethodExpressionParser()->parse($method, $args);
                $finderMethod = key($result);
                $conditions = $result[$finderMethod];
            } catch (MethodExpressionParserException $e) {
                $message = sprintf('%s: %s()', $e->getMessage(), $method);
                throw new EntityRepositoryException($message);
            }
            return $this->$finderMethod($conditions);
        }

        $message = 'Invalid method call: ' . __METHOD__;
        throw new BadMethodCallException($message);
    }
}
```

### **Performance**

PHP doesn't allow you to define methods dynamically, this means that every time you invoke a finder method the parser has to search, extract and map all the attribute names and expressions. To avoid introducing this performance overhead you can cache the attribute names. For example:

```
class EntityRepository
{
    private $methodExpressionParser;
    private $classMetadata;

    // Return a single instance of MethodExpressionParser
    public function getMethodExpressionParser() {
    }

    // Return a single instance of ClassMetadata
    public function getClassMetadata() {
    }

    // Invoke finder methods
    public function __call($method, $args) {
        if ('f' === $method{0}) {
            $parser = $this->getMethodExpressionParser();
            $classMetadata = $this->getClassMetadata();
            try {
                $finderMethod = $parser->determineFinderMethod($method);
                if ($classMetadata->hasMissingMethod($method)) {
                    $attributes = $classMetadata->getMethodAttributes($method);
                    $conditions = $parser->map($args, $attributes);
                } else {
                    $expressions = substr($method, strlen($finderMethod));
                    $attributes = $this->extractAttributeNames($expressions);
                    $conditions = $parser->map($args, $attributes);
                    $classMetadata->setMethodAttributes($method, $attributes);
                }
            } catch (MethodExpressionParserException $e) {
                $message = sprintf('%s: %s()', $e->getMessage(), $method);
                throw new EntityRepositoryException($message);
            }
            return $this->$finderMethod($conditions);
        }

        $message = 'Invalid method call: ' . __METHOD__;
        throw new BadMethodCallException($message);
    }
}
```

The Expression objects are lazy-loaded, depending on the expressions found in the method name.

### **Extensibility**

The MethodExpressionParser class was designed with extensibility in mind, allowing you to add new Expressions to the library.

```
abstract class Expression {
}
class EqualsExpression extends Expression {
}
```

### **Source Code**

Browse source code: [http://fedecarg.com/repositories/show/expressionparser](http://fedecarg.com/repositories/show/expressionparser)

Check out the current development trunk with:

```
$ svn checkout http://svn.fedecarg.com/repo/Zf/Orm
```
