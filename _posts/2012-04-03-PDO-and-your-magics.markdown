---
layout: post
tags: [pdo, vo, magic]
title: PDO and your magics - Part 1
author: Kinn Coelho Julião
email: kinncj@gmail.com
---
## Hello guys

In this article i'll show you the PDO _fetchObject_ working fine with a _VO_.

According to Martin Fowler, Eric Evans and other evangelists of a rich domain model, a Value Object is a simple object, usually with attributes that do not reference other objects, immutable and no identity, they are merely representative. 

In other words, a true value object - The object that is worth something. 

Examples:
*Number: is a typical example of a VO. Its value justifies its existence. It is immutable, because you can not change the values of a number. 
You must create a new to this. 
Their comparison is not just in all his attributes, comparing only the value which represents enough.
*Money: Philip Footwear, probably based on the example of Fowler, gives a great example of VO talking about the Money object. 
A money, we assume here, Dollar, has a value of one, five, ten, fifty, etc ... in real life, is a part. 
Obviously, change its value would result in erasure or problems with the police, thus justifying its immutability, however, if I borrow ten dollars, no need to receive the same pay ten dollars. 
It may be another note, both having the same value.
*Colour: Why, a color valley, herm ... a color! 
Why to represent colors is much better to use Enums, by the way, all or the vast majority of what PO should be possible to represent using Enumerators.


It gave great insight to understand the true VO? 

It represents a value, simple and intuitive, right? 

Very close to what we call primitive, after all, if the creators of programming languages, knew all VOs possible, we would not need to have objects explicitly declared for them, because our compilers recognize a simple USD 10.30 in the editor, and ten dollars and thirty cents.

But then, it was not quite what you thought was a VO? 

Well, I guess let's see: 

For you, a VO was a structure with getterns and setters, commonly used to carry values ​​between layers and layers. 

So, well this is the same confusion. 

This is the TO (Transfer Object) - an adaptation of the Data-Transfer Object pattern (also cataloged by Fowler) - The TO is actually a composite of several attributes, serialized (is transported between layers), serving mainly to minimize traffic objects in a network.

## Come on, let's do it!

At first time, i'll create a simple _VO_ called _UserVO_

{% highlight php %}
<?php
  //UserVO.php
  class UserVO{
    public $name,$email,$phone,$address; // We don't exactly need this... but i love to declare things.
        
  //declare anything else that you want here!  
  }
{% endhighlight %}

Like you can see, we have an _UserVO_ with _name_, _email_, _phone_ and _address_ attributes.

This is basicly a return from a UserDAO or a user table from your database.

### What's the magic?

Basicly, when we fetch some data from database, we'll tell to PDO to put's the result into this _VO_ ..
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

{% highlight php %}
<?php
  //Look, it's a poor php code, just to demonstrate for all u guys.
  //demo.php
  spl_autoload_register(function($className){
  require_once str_replace(array('\\','_'),'/',$className).'.php';
  //Yeah, and autoloader... not too poor
  });
  //I supose that u have a config object/array/something to your database credentials...
  //I'll not abstract this to a Proxy, cuz it's just a demo for the magic, not for patterns and others
  $pdo = new PDO("{$config->dbdriver}:host={$config->dbhost};dbname={$config->dbname}",$config->dbuser,$config->dbpass);
	
  $query = "SELECT * FROM users"; //Let's select the hole thing
	
  $result = $pdo->query($query); //Let's query it

  $userVO = $result->fetchObject("UserVO"); //Let's fetch into our _VO_

  var_dump($userVO); //Let's dump and see the result!
  /*
  * it should print it
  *object(UserVO)#5 (5) {
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

Example running [here](http://kinncj.com.br/kinn/phpedia/examples/2012-04-03-2012-04-03-PDO-and-your-magics/1/)

#### That's all folks!
