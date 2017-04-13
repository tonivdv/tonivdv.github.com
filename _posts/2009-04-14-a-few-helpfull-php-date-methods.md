---
title: A few helpful PHP date methods
author: kennethvr
layout: post
permalink: /2009/04/14/a-few-helpfull-php-date-methods/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085625
categories:
  - General
  - php
  - Symfony
tags:
  - date
  - datetime
  - methods
  - php
  - time
---
For a lot of people dates in PHP are an issue, and the PHP functions aren&#8217;t sufficient enough to do all converts you need.  
Therefore you find a few easy PHP date conversion methods. Feel free to add your own methods or requests in the comments as we know that these methods don&#8217;t cover everything. You can see it for yourself when you read the rest of this entry:

  * get start and end date for the next two months in an array
  * get the datetime for a specific user culture in Symfony
  * get all days between a given start and end date and return it in an array
  * get the difference between two dates

<!--more-->

  1. **get start and end date for the next two months in an array **<span style="text-decoration: underline;"><br /> input</span>: nothing  
    <span style="text-decoration: underline;">output</span>:  
    array[0] = 1239706452  
    array[1] = 1244976852  
    <span style="color: #339966;">*updated -> thx naholyr</span> <pre class="brush: php; title: ; notranslate" title="">/**
 * get the next two months starting from today.
 *
 * @return array
 */
 public function getNextTwoMonths(){
 return array(time(), strtotime(’+2 month’));
 }
</pre>

  2. **get the datetime for a specific user culture in Symfony**<span style="text-decoration: underline;"><br /> input</span>: 16/09/08 <span style="text-decoration: underline;"><br /> output</span>: 2008-09-16 23:35:55 <pre class="brush: php; title: ; notranslate" title="">/**
 * Parses a date String from a specific culture to a Date.
 *
 * input example: 16/09/08
 *
 * returns: DateTime (Date)
 * @param String $period
 * @return Array of DateTime objects
 */
 public function parseDate($period) {

 $userCulture =  sfContext::getInstance()-&gt;getUser()-&gt;getCulture();

 $timestamp = sfContext::getInstance()-&gt;getI18N()-&gt;getTimestampForCulture($period, $userCulture);

 $period = new DateTime(date('Y-m-d H:i:s', $timestamp));

 return $period;
 }
</pre>

  3. **get all days between a given start and end date and return it in an array.**<span style="text-decoration: underline;"><br /> input</span>: 2 DateTime dates<span style="text-decoration: underline;"><br /> output</span>: array of days  
    day[0] = 2008-12-10  
    day[1] = 2008-12-11 <pre class="brush: php; title: ; notranslate" title="">/**
 * With a given start and end date it will creates all the dates in between and return
 * it in an array.
 *
 * @param DateTime $startDate
 * @param DateTime $endDate
 * @return Array
 */
 public function getDaysByPeriod(DateTime $startDate, DateTime $endDate) {

 $startDt = clone $startDate;

 $days = array();

 while ( $startDt-&gt;format('U') &lt;= $endDt-&gt;format('U') ) {
 $days[] = $startDt-&gt;format('Y-m-d');
 $startDt-&gt;modify("+1 days");
 }

 return $days;
 }
</pre>

  4. **get the difference between two dates**<span style="text-decoration: underline;"><br /> input</span>: <span style="text-decoration: underline;"><br /> </span>2009-04-14 10:08:35  
    2009-04-15 10:10:35  
    <span style="text-decoration: underline;"> output</span>: unix timestamp of the difference  
    <span style="color: #339966;">* updated -> thx Toni and David</span> <pre class="brush: php; title: ; notranslate" title="">/**
 * get the difference between two dates
 *
 * @param date $firstTime
 * @param date $lastTime
 * @return integer
 */
public function timeDiff($firstTime,$lastTime)
{
 return strtotime($lastTime) - strtotime($firstTime);
}
</pre>