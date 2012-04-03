---
layout: post
tags: [pdo, vo, magic]
title: PDO and your magics(Part 1)
author: Kinn Coelho Julião
email: kinncj@gmail.com
---
## Hello guys

In this article i'll show you the PDO _fetchObject_ working fine with a _VO_.

***

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
Yeah, creazy hun?

Let's do some piece of code... 
At first we need a table, right?

######So

{% highlight %}

    CREATE TABLE users(id int not null primary key auto_increment, name text, email varchar(255), phone int(11), address text);

{% endhighlight %}

