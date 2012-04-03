---
layout: post
tags: [pdo, vo, magic]
title: PDO and your magics - Part 1
author: Kinn Coelho Julião
email: kinncj@gmail.com
---
## Hello guys

In this article i'll show you the PDO _fetchObject_ working fine with a _VO_.

At first time, i'll create a simple _VO_ called _UserVO_

{% highlight php %}
<?php
  //UserVO.php
  class UserVO{
    public $name,$email,$phone,$address; // We don't exactly need this... but i love to declare things.
        
  //declare anything else that you want here!  
  }
{% endhighlight %}

