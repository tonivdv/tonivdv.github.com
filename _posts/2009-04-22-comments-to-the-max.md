---
title: Comments to the max
author: kennethvr
layout: post
deprecated: true
permalink: /2009/04/22/comments-to-the-max/
syntaxhighlighter_encoded:
  - 1
categories:
  - General
  - Languages
  - php
tags:
  - code
  - comment
  - javadoc
  - phpdoc
---
I was wondering around on the big wide scary web and saw a lot of badly written comment for code so I thought it shouldn&#8217;t be bad to give a little overview on how comment should be written, at least how I think it should be written.

Not all comments can be written in the same way and not all comments can be read by other programs but I personally think the Java comment is the most readable one, and (huge advantage) can be read by the Javadoc processor. When writing in PHP this is done in the same way as PHPDoc is an adaptation of Javadoc.

So write your comment like this:

<pre class="brush: php; title: ; notranslate" title="">/**
 * This is a basic description of your method,
 * write down here what the function does in general
 *
 *@param integer $id
 *@return array Array of objects
 *@author Kenneth van Rumste
 */

</pre>

<!--more-->

As you can see we have the general description of a method first, written in a few words. Only write more if this is really necessary. Comments are there to help when the developer doesn&#8217;t understand the code and everybody should keep in mind that the name of a method should contain the basic information about what&#8217;s happing inside. For example don&#8217;t use methodName1 for a method, but retrieveDailyUsers to select the daily users. So you don&#8217;t need a lot of extra comment to explain what is happening, Jeff Atwood (Coding Horror) wrote a great post last year on the<a href="http://www.codinghorror.com/blog/archives/001150.html" target="_blank"> usage of comments</a>.

In the second part you can see a bunch of tags added into the comment, these are very handy when writing code as they inform parsers how to display documentation and allow IDE to define variable types.

I added only the basic data to the comment and a lot more tags can be added but as I m a lazy guy you can check them all out <a href="http://en.wikipedia.org/wiki/PHPDoc" target="_blank">here</a>[][1].

To finish this post let us take a look at some of my all time favorite, comment jokes

<pre class="brush: php; title: ; notranslate" title="">stop(); // Hammertime!

Catch (Exception e) {  
//who cares?
}   

Exception up = new Exception("Something is really wrong.");

throw up;  //ha ha   

doRun.run();  // ... "a doo run run".   

long long ago; /* in a galaxy far far away */   

/////////////////////////////////////// this is a well commented line

//Abandon all hope yea who enter beyond this point   

//Woulda
if(x) {}
//Shoulda
else if(y) {}
//Coulda
else {}   

/* This isn't the right way to deal with this,
but today is my last day, Ron just spilled coffee on my desk,
and I'm hungry, so this will have to do... */
return 12; // 12 is my lucky number   

#define TRUE FALSE //Happy debugging suckers   

// TODO - Comment this function
</pre>

 [1]: http://en.wikipedia.org/wiki/PHPDoc