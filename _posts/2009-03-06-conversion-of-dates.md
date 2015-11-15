---
title: conversion of dates
author: van Rumste Kenneth
layout: post
permalink: /2009/03/06/conversion-of-dates/
categories:
  - General
  - php
tags:
  - Add new tag
  - date
  - day
  - function
  - week
  - year
---
I struggled with a bit of code last week, but found a nice solution.

The goal was to convert 3 parameters into a date:

1. The day of the week (0 -> 6, 0 = Monday, 1 = Tuesday, 2 = Wednesday, etc.)  
2. The week number (0 – 53, <a title="windows bug in week numbers" href="http://www.devexp.eu/?p=385" target="_self">see our previous post for more info</a>)  
3. And the year (2009)

Well in this function we got to that result.

<pre class="brush: php; title: ; notranslate" title="">/**
* create datetime from dow, week number and year
* @param integer $dow (MUST BE 0 -&gt; 6)
* @param integer $weekNumber
* @param integer $year
* @return datetime
*/
public function getDateTimeByDowWeekYear ($dow, $weekNumber, $year) {
// get the first day of the current year, according iso standards
$offset = date('w', mktime(0,0,0,1,1,$year));
$offset = ($offset &lt; 5) ? 1-$offset : 8-$offset;
//get the first Monday of the year
$monday = mktime(0,0,0,1,1+$offset,$year);
//add the number of weeks
$mondayTime = strtotime('+' . ($weekNumber - 1) . ' weeks', $monday);
// add the number of weekdays
$dayTime = strtotime('+' . $dow . ' days', $mondayTime);
//create a date
return date('Y-m-d H:i:s',$dayTime);
}
</pre>

It seemed to be a lot easier then I thought.  
Et voila, have fun.