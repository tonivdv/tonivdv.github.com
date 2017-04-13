---
title: Windows bug in week numbers
author: kennethvr
layout: post
permalink: /2009/02/27/windows-bug-in-week-numbers/
dsq_thread_id:
  - 3852237971
categories:
  - General
  - php
tags:
  - bug
  - outlook
  - week number
  - windows
---
Today is a joyful day as we found a bug in the Windows Outlook Calendar.  
We are currently working with week numbers and wanted to know what the correct standard was for this subject, so we searched for the ISO Standard and we found this explanation:

> The ISO week-numbering year starts at the first day (Monday) of week 01 and ends at the Sunday before the new ISO year (hence without overlap or gap). It consists of 52 or 53 full weeks. The ISO week-numbering year number deviates from the number of the calendar year (Gregorian year) on a Friday, Saturday, and Sunday, or a Saturday and Sunday, or just a Sunday, at the start of the calendar year (which are at the end of the previous ISO week-numbering year) and a Monday, Tuesday and Wednesday, or a Monday and Tuesday, or just a Monday, at the end of the calendar year (which are in week 01 of the next ISO week-numbering year). For Thursdays, the ISO week-numbering year number is always equal to the calendar year number.

So for 01-01-2010 this will give us as result week 53.

We did a little test and came up with these results:  
PHP: 01-01-2010 &#8212; 53 weeks  
Windows Mobile Outlook: 01-01-2010 &#8212; 53 weeks  
Windows Vista Outlook 2007 SP1: 01-01-2010 – 1 week  
It seems that outlook doesn’t follow the ISO-standard… LOL

After a bit of research we found out that this so called bug isn’t a bug, but an option you can select (check full week as first week of the year). Damned, we thought we found a bug….