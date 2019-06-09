Utilisation et avantage des Interface en POO
===================



Interface
-------------

We know that an interface is defined by the interface keyword and all methods are abstract. All methods declared in an interface must be public; this is simply the nature of an interface. Here is the example :

```php
<?php
interface Logger
{
    public function execute();
}
```

In an interface the method body is not defined, just the name and the parameters.
If we do not use an interface what problems will happen? Why we should use interface? Herein you will get these answers. Please see the code in below:

```php
<?php
class LogToDatabase 
{
    public function execute($message)
    {
        var_dump('log the message to a database :'.$message);
    }
}

class LogToFile 
{
    public function execute($message)
    {
        var_dump('log the message to a file :'.$message);
    }
}

class UsersController 
{ 
    protected $logger;
    
    public function __construct(LogToFile $logger)
    {
        $this->logger = $logger;
    }
    
    public function show()
    { 
        $user = 'nahid';
        $this->logger->execute($user);
    }
}

$controller = new UsersController(new LogToFile);
$controller->show();
```

In the above example I do not use interface. I write to the log using the LogToFile class. But now if I want to write a log using LogToDatabase I have to change hard coded class reference in the above code on line number 23. That line code in below :

```php
public function __construct(LogToFile $logger)
```

This code will be

```php
public function __construct(LogToDatabase $logger)
```

In a large project, if I have multiple classes and a need to change, then I have to change all the classes manually. But If we use an interface this problem is solved; and we will not need to change code manually.


Now see the following code and try to realize what happened if I use interface:

```php
<?php
interface Logger 
{
    public function execute($message);
}

class LogToDatabase implements Logger 
{
    public function execute($message){
        var_dump('log the message to a database :'.$message);
    }
}

class LogToFile implements Logger 
{
    public function execute($message) 
    {
        var_dump('log the message to a file :'.$message);
    }
}

class UsersController 
{
    protected $logger;
    
    public function __construct(Logger $logger) 
    {
        $this->logger = $logger;
    }
    
    public function show() 
    {
        $user = 'nahid';
        $this->logger->execute($user);
    }
}

$controller = new UsersController(new LogToDatabase);
$controller->show();
```

Now If I change from LogToDatabase to LogToFile I do not have to change the constructor method manually. In the constructor method I have Injected an interface; not any arbitrary class. So If you have multiple classes and swap from one class to another class you will get the same result without changing any class references.

In the above example I write a log using LogToDatabase and now I want to write log using LogToFile, I can call it in this way

```php
$controller = new UsersController(new LogToFile);
$controller->show();
```

I get the result without changing other classes. Because the interface class handles the swapping issue.


Merci pour votre lecture ! 
