---
layout: post
tags: [unit testing, phpunit, static methods]
title: Testing static methods with PHPUnit
filename: 2012-06-08-testing-static-methods-with-phpunit.markdown

---

Let's talk about unit testing. Unfortunately, we tend to face a lot more untestable code than we would like to.
PHPUnit's creator, Sebastian Bergmann, has a lot of [posts](http://sebastian-bergmann.de/archives/883-Stubbing-and-Mocking-Static-Methods.html) showing how to test untestable code.

I would just like to share with you guys a very simple technique that can help you out when testing methods which depend on static calls.

To illustrate, let me show you a simple code snippet.

{% highlight php %}
<?php
 
class Repository
{
    public function saveUser() {}
}
 
class User {}
 
class Logger
{
    public static function logException() {}
}
 
class UserService
{
    protected $repository;
 
    public function setRepository(Repository $repository)
    {
        $this->repository = $repository;
    }
 
    public function getRepository()
    {
        return $this->repository;
    }
 
    public function addUser(User $user)
    {
        try {
            $this->getRepository()->saveUser($user);
        } catch (Exception $e) {
            Logger::logException($e);
            return false;
        }
 
        return true;
    }
}
 
class UserServiceTest extends PHPUnit_Framework_TestCase
{
    private $repository;
    private $userService;
 
    public function setUp()
    {
        $this->repository = $this->getMock('Repository');
        $this->userService = new UserService();
 
        $this->userService->setRepository($this->repository);
    }
 
    public function testAddUserSuccessfully()
    {
        $this->repository->expects($this->once())
            ->method('saveUser')
            ->with($this->isInstanceOf('User'))
            ->will($this->returnValue(true));
 
        $this->assertTrue($this->userService->addUser(new User()));
    }
 
    public function testAddUserExceptionThrown()
    {
        $this->repository->expects($this->once())
            ->method('saveUser')
            ->with($this->isInstanceOf('User'))
            ->will($this->throwException(new Exception()));
 
        $this->assertFalse($this->userService->addUser(new User()));
    }
}
{% endhighlight %}

Notice that on method `UserService::addUser()` when an *Exception* is thrown, a log needs to be generated. For doing that, we have a static method `Logger::logException()`.

A very simple way of mocking static methods is to use `staticExpects` from PHPUnit. However, how do we inject this mocked class?
Well, you can simply create *getters and setters* for injecting the mocked class. After injecting the class, the static mocked method will be called, therefore, making it testable.

{% highlight php %}
<?php
 
class Repository
{
    public function saveUser() {}
}
 
class User {}
 
class Logger
{
    public static function logException() {}
}
 
class UserService
{
    protected $repository;
    protected $logger;
 
    public function setRepository(Repository $repository)
    {
        $this->repository = $repository;
    }
 
    public function getRepository()
    {
        return $this->repository;
    }
 
    public function setLogger($logger)
    {
        $this->logger = $logger;
    }
 
    public function getLogger()
    {
        if (!$this->logger) {
            $this->logger = 'Logger';
        }
 
        return $this->logger;
    }
 
    public function addUser(User $user)
    {
        $logger = $this->getLogger();
 
        try {
            $this->getRepository()->saveUser($user);
        } catch (Exception $e) {
            $logger::logException($e);
            return false;
        }
 
        return true;
    }
}
 
class UserServiceTest extends PHPUnit_Framework_TestCase
{
    private $repository;
    private $userService;
    private $logger;
 
    public function setUp()
    {
        $this->repository = $this->getMock('Repository');
        $this->logger = $this->getMock('Logger');
        $this->userService = new UserService();
 
        $this->userService->setRepository($this->repository);
        $this->userService->setLogger($this->logger);
    }
 
    public function testAddUserSuccessfully()
    {
        $this->repository->expects($this->once())
            ->method('saveUser')
            ->with($this->isInstanceOf('User'))
            ->will($this->returnValue(true));
 
        $logger = $this->logger;
        $logger::staticExpects($this->never())
            ->method('logException');
 
        $this->assertTrue($this->userService->addUser(new User()));
    }
 
    public function testAddUserExceptionThrown()
    {
        $this->repository->expects($this->once())
            ->method('saveUser')
            ->with($this->isInstanceOf('User'))
            ->will($this->throwException(new Exception()));
 
        $logger = $this->logger;
        $logger::staticExpects($this->once())
            ->method('logException')
            ->with($this->isInstanceOf('Exception'));
 
        $this->assertFalse($this->userService->addUser(new User()));
    }
}
{% endhighlight %}

By doing that, you can not only mock static calls, but also let your class fully functional.
Indeed, there are several other ways of mocking and injecting static calls. This is just one of them.
Thanks.
