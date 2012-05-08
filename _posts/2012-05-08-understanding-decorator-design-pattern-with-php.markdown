---
layout: post
tags: [Decorator, Design pattern]
title: Understanding the decorator design pattern
filename: 2012-05-08-understanding-decorator-design-pattern-with-php.markdown

---
When we read the name of the pattern Decorator would imagine that silly name, meaningless. Good is not as meaningless as well.

The Decorator works exactly as the name describes.

##Definition:


Decorator is a design pattern software that allows you to add a behavior to an existing object at runtime, or dynamically adds additional responsibilities to an object.

This pattern adds responsibilities dynamically to a class, generating more flexible alternative for certain classes use subclasses through inheritance or composition.

Using Decorator, we have more momentum in the inheritance, we can use inheritance without the problem of transforming our class's into God class's.

To help us understand the pattern, let's create a scenario and a code example.

##Scenario


Imagine we are developing a banking system where an account, you can transfer funds to accounts or savings accounts.

##We have a problem to solve:


When we make the transfer money, we have an account and savings account. Following the pattern of Object Orientation, we must respect the division of responsibility.

So the account should be a class and savings account should be another class.

Our problem is to credit the account values ​​and define how we will do it in a clean and easy to maintain.

The Decorator can help us do just that, he will add responsibility receiveFounds() in our class at the right time, including making the validation that determine what kind of account value will be credited. Here's an example:

###Account.php

{% highlight php %}
< ?php
	class Account extends Decorator{

		public $numAccount;
		public $numAtualFounds;

		public function __construct() {
			$this->numAccount 	= 0;
			$this->numAtualFounds	= 0;

			return $this;
		}

		public function transferFounds( $numAccount , $transferValue ) {
			$operationSuccess = false;

			$operationSuccess = parent::doFoundTransfer( $this, $numAccount , $transferValue );

			return $operationSuccess;
		}

		public function receiveTransferredFounds( Account $numPayAccount , $numAccount , $transferValue ) {
			// implementation

			$result = "Account transfer successful";

			return $result;
		}
	}
?>
{% endhighlight %}

###SavingsAccount.php

{% highlight php %}
< ?php
	class SavingsAccount extends Decorator{

		public function receiveTransferredFounds( Acount $numPayAccount , $numAcount , $transferValue ) {
			// implementation

			$result = "Savings Acount transfer successful";

			return $result;
		}
	}
?>
{% endhighlight %}

###Decorator.php

{% highlight php %}
< ?php
	class Decorator{
		private $DGS_VERIFY_SAVINGS_ACCOUNT = 500;

		protected function doFoundTransfer( Account $numPayAccount , $numAccount  , $transferValue ) {
			$account = "";

			if( $this->validateSavingsAccount( $numAccount ) ) {
				$account = new SavingsAccount();
			}
			else {
				$account = new Account();
			}

			return $account->receiveTransferredFounds( $numPayAccount , $numAccount , $transferValue );
		}

		private function validateSavingsAccount( $numAccount ) {
			if( substr( $numAccount , -3 , 3 )  == $this->DGS_VERIFY_SAVINGS_ACCOUNT ) {
				return true;
			}
			else {
				return false;
			}
		}
	}
?>
{% endhighlight %}

###Implementation

{% highlight php %}
< ?php
	require_once("Decorator.php");
	require_once("Account.php");
	require_once("SavingsAccount.php");	

	$account = new Account();
	$account->numAccount = 26558454200;

	// transferencia para conta corrente
	$transferResult = $account->transferFounds( 556558474154 , 6000.00 );

	echo("<p>" . $transferResult . "</p>");

	// transferencia para conta poupança
	$transferResult = $account->transferFounds( 556558474500 , 2365.50 );

	echo("<p>" . $transferResult . "</p>");
?>
{% endhighlight %}

Well as we saw above, we made two payments to two different accounts. No need to report what kind of account would be without having to instantiate anything more.

In the instance of our business, we transfer the responsibility and <strong>Decorator</strong> entered correctly in our method of transferring funds.

This pattern is very simple and very easy to use.

I hope you enjoyed, and good until the next code.

This article was taken from my blog: [The Blog of Paulo Teixeira](http://www.pauloteixeira.blog.br/site/en/content/2012/05/understanding-decorator-design-pattern-with-php/).
