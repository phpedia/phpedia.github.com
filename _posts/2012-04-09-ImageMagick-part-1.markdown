---
layout: post
tags: [imagemagick, digital asset, image]
title: ImageMagick - A great tool to work with digital assets - Part 1
filename: 2012-04-09-ImageMagick-part-1.markdown

---
## Image? Magick?
ImageMagick is a software suite to create, edit, compose or convert digital assets.

It can read and write assets in a variety of formats like JPEG, GIF, PNG, TIFF and others like PSD, AVI, PDF.

## For what?

You can use ImageMagick to resize, flip, mirror, rotate, distort, shear, adjust image colors, apply special effects, draw text, lines, eclipses, convert video files and more.

## How it works?

ImageMagick is typically utilized from the command line, but you can use the features from programs written in your favorite language.

## Can i use it from PHP?

Sure that!

You can use everything that you want in your PHP... 

At first moment, we think about _exec_, but in ImageMagick case, we have three extensions that allow us to work without use the _exec_

* [MagickWand for PHP](http://www.magickwand.org/)
* [Imagick](http://pecl.php.net/package/imagick)
* [phMagick](http://www.francodacosta.com/phmagick)

### Which one do we use?

Imagick

### Why?

Because it's a native php extension (that you can install via pecl if your PHP binary doesn't have it) and uses the ImageMagick API.

### What we need?

We will need ImageMagick version 6.2.4+ (better 6.5.7+, becouse we can use the profile methods in this version) and PHP 5.1.3+

### Let's start

_I assume you have installed the extension via PECL_

The first thing we should do is find an image to work, like [this one](http://kinncj.com.br/kinn/phpedia/examples/2012-04-07-ImageMagick/1/mini_500_16800_1283811426672964.jpg)

Basicly, we need to define what we want to do with the image.

So, to start to show you the API, i want to change this image into grayscale

* Here, we have an Imagick object. When we instantiate an Imagick object, we will pass the file handle to the constructor method *

{% highlight php %}
<?php
$imagick = new Imagick("mini_500_16800_1283811426672964.jpg");
{% endhighlight %}

* Now, we need to change its colorspace to a grayscale colorspace. *

* Note: When you are working with [ColorSpace](http://en.wikipedia.org/wiki/Color_space), you must be careful with [ColorProfiles](http://en.wikipedia.org/wiki/ICC_profile) *

{% highlight php %}
<?php
$imagick->setImageColorSpace(Imagick::COLORSPACE_GRAY);
{% endhighlight %}

* And now, we just need to save a new image *

{% highlight php %}
<?php
$imagick->writeImage("My_new_image.jpg");
{% endhighlight %}

### Can i change the file format?

Sure that you can!
Let's see the snippet

{% highlight php %}
<?php
$imagick->setImageFormat("png");
$imagick->writeImage("My_new_image.png");
{% endhighlight %}

_now we have a PNG file_

### Can i write in this image?

Let's see how can we do this..

At first, we will need the Imagick object... we have it!
{% highlight php %}
<?php
$imagick = new Imagick("mini_500_16800_1283811426672964.jpg");
{% endhighlight %}

At second, we will need an ImagickDraw object.
{% highlight php %}
<?php
$draw = new ImagickDraw();
{% endhighlight %}

Now we can start to write

We need to get the image height, to calculate the position of our string
{% highlight php %}
<?php
$height = $imagick->getImageHeight();
{% endhighlight %}

We need to set the color too
{% highlight php %}
<?php
$draw->setFillColor("#333333");
{% endhighlight %}

Set the font size
{% highlight php %}
<?php
$draw->setFontSize(14);
{% endhighlight %}

Set the text under color
{% highlight php %}
<?php
$draw->setTextUnderColor("#FF0000");
{% endhighlight %}

Write some text in the image
{% highlight php %}
<?php
$imagick->annotateImage($draw, 80, $height - 10,0,"Dark side of the force");
{% endhighlight %}

And save the new file
{% highlight php %}
<?php
$imagick->writeImage("My_image.jpeg");
{% endhighlight %}

_darkside.php_

This must be your script

{% highlight php %}
<?php
$imagick = new Imagick("mini_500_16800_1283811426672964.jpg");
$draw = new ImagickDraw();
$height = $imagick->getImageHeight();
$draw->setFillColor("#333333");
$draw->setFontSize(36);
$draw->setTextUnderColor("#CCCCCC");
$imagick->annotateImage($draw, 80, $height - 10,0,"Dark side of the force");
$imagick->writeImage("My_image.jpeg");
{% endhighlight %}

And this one a real [dark side image](http://kinncj.com.br/kinn/phpedia/examples/2012-04-07-ImageMagick/1/)
{% highlight php %}
<?php
$imagick = new Imagick("mini_500_16800_1283811426672964.jpg");
$draw = new ImagickDraw();
$height = $imagick->getImageHeight();
$draw->setFillColor("#333333");
$draw->setFontSize(36);
$draw->setTextUnderColor("#CCCCCC");
$imagick->setImageColorSpace(Imagick::COLORSPACE_GRAY);
$imagick->writeImage('My_grayscale_image.jpeg');
$imagick = new Imagick("My_grayscale_image.jpeg");
$imagick->annotateImage($draw, 80, $height - 10,0,"Dark side of the force");
$imagick->writeImage("My_darkside_image.jpeg");
header('Content-Type: image/jpeg');
readfile("My_darkside_image.jpeg");
{% endhighlight %}

## That's it!