---
title: Arrays or objects in PHP?
author: van Rumste Kenneth
layout: post
permalink: /2008/09/25/arrays-or-objects-in-php/
categories:
  - php
tags:
  - array
  - object
---
I assumed until a couple of days ago that objects are way better then arrays, but let’s take a closer look into that.

As objects are an instance of a class you can always know what type of data is inserted in the object, when trying this in an array you quickly get in to problems, for example:

<pre class="brush: php; title: ; notranslate" title="">$testcase = array();
$testcase["Name"] = "testcase";
$testcase["value"] = 20;</pre>

When doing this in objects, you can split it up very easy like so:

<pre class="brush: php; title: ; notranslate" title="">$testcase = new testcaseObject();
$testcase-&gt;setName("testcase");
$testcase-&gt;setValue(20);</pre>

I guess you see my point….

When using huge amount of data though, arrays seem to have the upper hand in memory usage so that can count as an advantage.

When we create a query to get some data from a database that returns us multiple rows, the best thing you can do is make an array of objects. Like this:

<pre class="brush: php; title: ; notranslate" title="">while( statement ) {

$id = $row-&gt;categoryId;

// If category doesn't exist, we create him, else do nothing
if (!isset($category[$id])) {
$category[$id] = new testcaseCategory ();
$category[$id]-&gt;setId($id);
$category[$id]-&gt;setName($queryrow[categoryName]);
}

// Create testcaseObject
$testcase = new testcaseObject();
$testcase-&gt;setId($queryrow[id]);
$testcase-&gt;setValue($queryrow[name]);

// Add testcaseObject to testcaseCategory.
$category[$id]-&gt;addTestCaseObject($testcase);

}</pre>

Have fun with it.