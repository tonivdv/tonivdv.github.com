---
title: forgotten syntaxes for MySQL
author: kennethvr
layout: post
deprecated: true
permalink: /2009/03/31/forgotten-syntaxes-for-mysql/
syntaxhighlighter_encoded:
  - 1
categories:
  - database
  - General
  - mysql
tags:
  - delayed
  - duplicate key
  - infile
  - insert
  - mysql
  - option
  - outfile
  - replace
  - select
  - syntax
  - update
---
<img class="alignright" style="border: 0pt none; margin: 5px;" title="MySQL" src="http://dev.mysql.com/common/logos/logo_mysql_sun_a.gif" alt="" width="114" height="68" />A lot of people seem to have problems with updating or inserting data in tables in an intelligent way. We shouldn&#8217;t point a finger to those who don’t use the correct syntax for the problem it solves, because there are a lot of different ways to do this, but I wanted to create a little list of interesting syntax&#8217;s that are often forgotten and never used. I will talk about:

  * insert&#8230; on duplicate key update
  * replace
  * insert&#8230; select
  * Load data infile and select into outfile
  * delayed

<!--more-->

When starting with SQL a few years ago I always executed a query to get a row out of a table and then updated or insert a row in a second query. Of course there are better solutions for this problem according to the specific action you want to do:

**INSERT&#8230; ON DUPLICATE KEY UPDATE**  
In this case it is possible for anyone to insert a row in a table, but if the key of this row already exists, it updates a specific value:

<pre class="brush: sql; title: ; notranslate" title="">INSERT INTO table (a,b,c) VALUES (1,2,3) ON DUPLICATE KEY UPDATE c=c+1;</pre>

**REPLACE**  
Replace works exactly like insert, except that if a row already exists, for the unique index or primary key, it is first deleted and then inserted as a new row.  
If you have a table where there is no primary key or index, the replace statement makes absolutely no sense, as there will be nothing to delete.

<pre class="brush: sql; title: ; notranslate" title="">REPLACE INTO table_1 SET a=1, b=2;</pre>

The difference between on duplicate key and replace is:  
‘Insert … on duplicate key update’ does an insert or an update.  
‘Replace’ does and insert with a possible delete first.

**INSERT … SELECT**  
This will insert rows from one or more tables into another table quickly, again to avoid the fact that you should write to queries to execute this.  
Use ‘ignored’ in the syntax to ignore duplicate-key violations

**LOAD DATA INFILE and SELECT … INTO OUTFILE**  
‘Load data infile’ will read data from a file into table at very high speed and ‘select into outfile’ will do the exact opposite, writing data from a table to a file.

**DELAYED**  
When you have users on your application that don’t have the time to wait this is a very handy option for the insert statement. It is generally known that the insert statement takes a lot of time during the execution of it, and with this option you can create the opportunity for your user to directly go on with other actions. The insert will only be queued until the table isn’t being used by another thread. Another benefit is that huge inserts can be executed at once and this will result in a much faster execution time.