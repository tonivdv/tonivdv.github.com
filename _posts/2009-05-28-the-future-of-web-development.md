---
title: the future of web development
author: kennethvr
layout: post
permalink: /2009/05/28/the-future-of-web-development/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3829073107
categories:
  - General
  - html
  - Javascript
  - php
tags:
  - html 5
  - JavaScript 2.0
  - john resig
  - Jon Von Tetzchner
  - PHP 6
---
It is always hard to tell what the future will bring, especially in a world where everything is evolving so quickly. But it&#8217;s always good to know what libraries, languages or applications will influence the way we work or the language we code in. There for I took the time to create an overview of what could be interesting in the near and far future.  If you have any remarks or other ideas, I would be happy to hear them in the comments.

**HTML 5**

Jon Von Tetzchner (director of Opera) recently announced that HTML 5 will replace Flash almost completely. Will it go that far? I don&#8217;t think so, but what we will be able to do with it is quite impressive!

  * Extension of the DOM with new elements like: section, audio, video, canvas, etc&#8230;
  * New attributes like: ping, charset, async
  * Id, tabindex and repeat for every element

As you can see the HTML 5 will be an expansion of HTML 4 but with rather important differences. We can expect HTML 5 to get its W3C Candidate Recommendation in 2012 but it will be able to use some of the features sooner. One of those is canvas that is already a widely implemented element.

Serge Jespers wrote a post on his weblog as a reaction on what Jon Von Tetzchner said and you can read about it here <http://www.webkitchen.be/2009/05/27/adobe-versus-the-open-web/> He is an Adobe platform evangelist and Adobe pays his payckeck every month, but I do think the guy has a point.

**JavaScript 2.0  
**

With JavaScript being more popular than ever this is certainly worth taking a look at. John Resig created a slideshow on what will be changed in the 2.0 version and I must say that it looks promising.



**PHP 6**

A lot of people swear by the use of PHP over other language (like me <img src="http://www.devexp.eu/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> ) and a lot of other people say that PHP just isn’t sufficient enough. They are probably right because there are some insufficiencies in PHP, but in version 6 a few of those will be reviewed and adjusted. For now v6 is available as a developer snapshot so if you want you can already try it out.

Improved Unicode support.  
This was a really necessary to keep up with other languages like JAVA that have a better i18n (internationalization) support, in this way PHP will close up the gap it had and support you with a much broader set of characters.

Namespaces  
Namespaces are a way of avoiding name collisions between functions and classes without using prefixes in naming conventions that make the names of your methods and classes unreadable. By using namespaces we can never have the problem that 2 classes with the same name interfere with each other. I’m still wondering if this is the best solution but we’ll have to find out in the future. I’m only struggling with the idea that this only moves the problem to a higher level, as you now have to make sure your namespaces aren’t the same.

XML  
XMLReader and XMLWriter will become part of the PHP core, which will make it easier to work with XML in PHP.

Magic\_quotes and register\_globals will be removed from PHP as they are a huge problem in PHP security.

It is from version 6 required to use tags as the ASP-style tags are no longer supported (<% %>)

Those are the biggest changes, again, just like in JavaScript some of these changes are already implemented in a PHP5.3  
So in a nutshell, I do believe that PHP has a superb future in front of it, but that is also because of frameworks like Symfony, CakePhp, Zend, etc.