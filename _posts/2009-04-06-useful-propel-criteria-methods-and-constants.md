---
title: Useful Propel criteria methods and constants
author: van Rumste Kenneth
layout: post
permalink: /2009/04/06/useful-propel-criteria-methods-and-constants/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085594
categories:
  - Framework
  - General
  - mysql
  - Symfony
tags:
  - criteria
  - method
  - propel
  - query
---
I&#8217;m working with criteria quite often lately and must say it is a handy way of query writing. The only problem I have with criteria is that I don&#8217;t seem to find a simple overview (list) of the most important methods you can add to it. I don&#8217;t really like the <a href="http://propel.phpdb.org/trac/" target="_blank">Propel website</a> as I don&#8217;t find the thing I need in a few seconds and that is a must for a lot of people. If I create a list of the most important criteria methods for myself, I rather share it with you guys&#8230;

**Simple Select query **with 2 criteria to check:

<pre class="brush: php; title: ; notranslate" title="">$c = new Criteria();
$c-&gt;add(AuthorPeer::FIRST_NAME, "Karl");
$c-&gt;add(AuthorPeer::LAST_NAME, "Marx", Criteria::NOT_EQUAL);
$authors = AuthorPeer::doSelect($c);
// $authors contains array of Author objects
</pre>

In SQL this will be:

<pre class="brush: php; title: ; notranslate" title="">SELECT ... FROM author WHERE author.FIRST_NAME = 'Karl' AND author.LAST_NAME &lt;&gt; 'Marx';</pre>

It&#8217;s quite simple to write the criteria, the only thing needed to write them is a list of options.

<!--more-->

**To add Or to the criteria** that have to be checked write:

<pre class="brush: php; title: ; notranslate" title="">$cton1 = $c-&gt;getNewCriterion(AuthorPeer::FIRST_NAME, 'Karl');
$cton2 = $c-&gt;getNewCriterion(AuthorPeer::LAST_NAME,  'Marx', Criteria:: NOT_EQUAL);

// combine them
$cton1-&gt;addOr($cton2);
</pre>

In SQL this will be:

<pre class="brush: php; title: ; notranslate" title="">SELECT ... FROM author WHERE author.FIRST_NAME = 'Karl' OR author.LAST_NAME &lt;&gt; 'Marx';</pre>

Quite simple isn&#8217;t it, and the usage of criteria is quite readable once you start knowing all the options.

Possible operators for criteria (write them like Criteria::Equals)

<ul type="disc">
  <li>
    EQUAL (default)
  </li>
  <li>
    NOT_EQUAL
  </li>
  <li>
    GREATER_THAN
  </li>
  <li>
    LESS_THAN
  </li>
  <li>
    GREATER_EQUAL
  </li>
  <li>
    LESS_EQUAL
  </li>
  <li>
    LIKE
  </li>
  <li>
    NOT_LIKE
  </li>
  <li>
    IN
  </li>
  <li>
    CUSTOM
  </li>
</ul>

This allows you to write your own condition as second parameter.

<ul type="disc">
  <li>
    CUSTOM_EQUAL
  </li>
</ul>

This is used to write a custom condition in an UPDATE query

<ul type="disc">
  <li>
    ISNULL
  </li>
  <li>
    ISNOTNULL
  </li>
</ul>

For some methods you need to add a line to the criteria to specify if they have to be used or not:

**$c->setIgnoreCase**(true);  
This is to specify if the query should be case sensitive or not.

**$c->addJoin**(ReviewPeer::BOOK\_ID, BookPeer::ID, Criteria::INNER\_JOIN);  
INNER JOIN (default) the table in parameter 1 to the table in parameter 2. In the same way you can RIGHT or LEFT JOIN.

**$c->addAscendingOrder**ByColumn(table::column);  
This will add an ascending order for the specified column. Off course you can to the opposite too by using addDescendingOrderByColumn.

**$criteria->addSelectColumn**(self::LABEL);  
This will add only this column to the select statement, by default he selects all fields of a table (default *)

All info about the methods and constants of criteria can be found <a href="http://propel.phpdb.org/docs/api/1.3/runtime/propel-util/Criteria.html" target="_blank">here</a>.