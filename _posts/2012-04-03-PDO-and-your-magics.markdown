---
layout: post
tags: [pdo, dto, magic]
title: PDO and it's magics - Part 1
author: Kinn Coelho Julião
email: kinncj@gmail.com
---
## Hello guys

In this article i'll show you the PDO _fetchObject_ working fine with a _DTO_.

According to Martin Fowler, Eric Evans and other evangelists of a rich domain model, a Data Transfer Object is an object that carries data between processes in order to reduce the number of method 

### Many people in the Sun community use the term "Value Object" for this pattern. I use it to mean something else - Martin Fowler. 

_"Some people use it for DDD"_


When you're working with a remote interface, such as Remote Facade , each call to it is expensive. 

As a result you need to reduce the number of calls, and that means that you need to transfer more data with each call. 

The solution is to create a Data Transfer Object that can hold all the data for the call.


It represents a data... a value... in only one transfer... simple and intuitive, right? 

When we get this information from the Sun community, like you can see above, we can be confused with some "patterns" below:

#### VO

Very close to what we call primitive, after all, if the creators of programming languages, knew all VOs possible, we would not need to have objects explicitly declared for them, because our compilers recognize a simple USD 10.30 in the editor, and ten dollars and thirty cents.

#### TO

For you, a VO was a structure with getterns and setters, commonly used to carry values ​​between layers and layers. 

So, well this is the same confusion. 

**This is the TO (Transfer Object) - an adaptation of the Data-Transfer Object pattern (also cataloged by Fowler) - The TO is actually a composite of several attributes, serialized (is transported between layers), serving mainly to minimize traffic objects in a network.**

## Come on, let's do it!

At first time, i'll create a simple _DTO_ called _UserDTO_

{% highlight php %}
<?php
  class UserDTO{
    public $name,$email,$phone,$address; // We don't exactly need this... but i like to declare things.
        
  //declare anything else that you want here!
  }
{% endhighlight %}

Like you can see, we have an _UserDTO_ with _name_, _email_, _phone_ and _address_ attributes.

This is basicly a return from a UserDAO or a user table from your database.

### What's the magic?

Basicly, when we fetch some data from database, we'll tell to PDO to put's the result into this _DTO_ ..
Yeah, _crazy_ _hun_?

Let's do some piece of code...

At first we need a table, right?

###### So

{% highlight sql %}
CREATE TABLE users(id int not null primary key auto_increment, name text, email varchar(255), phone int(11), address text);
{% endhighlight %}

* The phone is int(11) becouse in Brazil it have about 11 digits, 011 99999999
* email is varchar(255) cuz i dont think someone has a bigger email address than it.
* name is text cuz people have big names ;)
* address is text... cuz, it's a full address

{% highlight sql %}
INSERT INTO users(name,email,phone,address) VALUES('test user','user@localhost',1199999999,'São Paulo');
{% endhighlight %}

populate it! _Come_ _at_ _me_ _Bro_ !


###### And our php code

Let's register an autoloader. So we have spl_autoload_register

In this way, we can register a lot of autoloaders in our application... we can register the Doctrine autoloader, the Twig autoloader, and others...
{% highlight php %}
<?php
  spl_autoload_register(function($className){
    require_once str_replace(array('\\','_'),'/',$className).'.php';
  });
{% endhighlight %}

I supose that u have a config object/array/something to your database credentials

I'll not abstract this to a Proxy, cuz it's just a demo for the magic, not for patterns and others
{% highlight php %}
<?php
  $pdo = new PDO("{$config->dbdriver}:host={$config->dbhost};dbname={$config->dbname}",$config->dbuser,$config->dbpass);
{% endhighlight %}

Let's select the whole thing
{% highlight php %}	
<?php
  $query = "SELECT * FROM users";
{% endhighlight %}

Let's query it

I'll not use prepared statements in this demo, cuz it's just a demo and with this strict select "SELECT * FROM users" we won't have problems
{% highlight php %}
<?php
  $result = $pdo->query($query); 
{% endhighlight %}

Let's fetch into our _DTO_
{% highlight php %}
<?php
  $userDTO = $result->fetchObject("UserDTO");
{% endhighlight %}

Let's dump and see the result!
{% highlight php %}
<?php
  var_dump($userDTO);
  /*
  * it should print it
  *object(UserDTO)#5 (5) {
  * ["name"]=>
  * string(9) "test user"
  * ["email"]=>
  * string(14) "user@localhost"
  * ["phone"]=>
  * string(10) "1199999999"
  * ["address"]=>
  * string(10) "São Paulo"
  * ["id"]=>
  * string(1) "1"
  *}
  */	
{% endhighlight %}

_demo.php_

*it must be your demo file*

{% highlight php %}
<?php
  spl_autoload_register(function($className){
  require_once str_replace(array('\\','_'),'/',$className).'.php';
  });
  $pdo = new PDO("{$config->dbdriver}:host={$config->dbhost};dbname={$config->dbname}",$config->dbuser,$config->dbpass);
  $query = "SELECT * FROM users";
  $result = $pdo->query($query); 
  $userDTO = $result->fetchObject("UserDTO"); 
  var_dump($userDTO);	
{% endhighlight %}

You can *try* it running the code into [writecodeonline](http://writecodeonline.com/php/) or directly in [this link](http://kinncj.com.br/kinn/phpedia/examples/2012-04-03-2012-04-03-PDO-and-your-magics/1/) 


#### That's all folks!
