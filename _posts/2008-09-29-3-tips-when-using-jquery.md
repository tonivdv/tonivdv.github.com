---
title: 3 tips when using jQuery
author: kennethvr
layout: post
deprecated: true
permalink: /2008/09/29/3-tips-when-using-jquery/
categories:
  - Javascript
  - Jquery
tags:
  - form
  - Jquery
  - json
  - live
  - query
  - submit
  - tips
---
**AjaxForm or ajaxSubmit**

<p style="padding-left: 30px;">
  This plug-in provides numerous options you can call to manage your form before and after submission and all this in AJAX. More info on <a href="http://malsup.com/jquery/form/">http://malsup.com/jquery/form/</a>
</p>

**Live Query**

<p style="padding-left: 30px;">
  When an element is loaded into a page, you will not be able to use the events on this element. But when the Live Query is used, this magically appears to work&#8230; This is because the Live Query binds events of elements together every few milliseconds and does a DOM (<em>Document Object Model)</em> update. A really cool plug-in that creates the possibility for web developers to execute events without reloading the page every time an element is clicked. The only disadvantage of live query is the fact that it appears to slow down the execution time a bit, but when you don&#8217;t have to reload your page each time, I guess it&#8217;s worth the usage. More info on <a href="http://brandonaaron.net/docs/livequery/">http://brandonaaron.net/docs/livequery/</a>
</p>

**JSON** (**J**ava**S**cript **O**bject **N**otation)

<p style="padding-left: 30px;">
  In our case we are using JSON as response-type from an AJAX request. This is a sort of XML but a lot easier to read, especially when you work with a lot of data. For more information on JSON visit <a href="http://json.org/">http://json.org/</a> or <a href="http://borkweb.com/story/the-case-for-json-what-is-it-and-why-use-it">http://borkweb.com/story/the-case-for-json-what-is-it-and-why-use-it</a>
</p>