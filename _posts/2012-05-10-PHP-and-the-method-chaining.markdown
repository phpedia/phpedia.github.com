---
layout: post
tags: [Design Pattern,Architeture]
title: PHP and the Pattern Method Chaining
filename: 2012-05-10-PHP-and-the-method-chaining.markdown
---
As the name implies, chaining methods, this technique allows multiple calls to invoke methods and each method returns an object, possibly containing the object itself.

With Method Chaining within our class that record setters have their values ​​and return the object.

It's simple and easy to apply to the examples we will:

##People.php

{% highlight php %}
<?php
	class People {
		private $name;
		private $lastname;
		private $age;
		
		public function __construct() {
			$this->name			="";
			$this->lastname		= "";
			$this->age			= 0;
			
			return $this;
		}
		
		public function setName( $val ) {
			$this->name = $val;
			
			return $this;
		}
		
		public function setLastname( $val ) {
			$this->lastname = $val;
			
			return $this;
		}
		
		public function setAge( $val ) {
			$this->age = $val;
			
			return $this;
		}
		
		public function introduce() {
			echo( "Name: " . $this->name . " Lastname: " . $this->lastname . "<br />Age: " . $this->age );
		}
		
		public function main( $name , $lastname , $age ) {
			$people = new People();
			$people ->setName( $name )
					->setLastname( $lastname )
					->setAge( $age )
					->introduce();
		}
	}
?>
{% endhighlight %}

Doing the implementation like this: 

{% highlight php %}
<?php
	$people = new People();
	$people->main( "Paulo" , "Teixeira" , 33 );
?>
{% endhighlight %}

If we do not use the standard chaining, we would do the class as follows:

##People.php

{% highlight php %}
<?php
	class People {
		private $name;
		private $lastname;
		private $age;
		
		public function __construct() {
			$this->name			="";
			$this->lastname		= "";
			$this->age			= 0;
			
			return $this;
		}
		
		public function setName( $val ) {
			$this->name = $val;
		}
		
		public function setLastname( $val ) {
			$this->lastname = $val;
		}
		
		public function setAge( $val ) {
			$this->age = $val;
		}
		
		public function introduce() {
			echo( "Name: " . $this->name . " Lastname: " . $this->lastname . "<br />Age: " . $this->age );
		}
		
		public function main( $name , $lastname , $age ) {
			$this->setName( $name )
			$this->setLastname( $lastname )
			$this->setAge( $age )
			$this->introduce();
		}
	}
?>
{% endhighlight %}

Doing the implementatio the same way, let's to see:

{% highlight php %}
<?php
	$people = new People();
	$people->main( "Paulo" , "Teixeira" , 33 );
?>
{% endhighlight %}

Ok folks, that's it! Enjoy!
