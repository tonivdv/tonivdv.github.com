---
title: Storing multiple data in 1 database field with PHP serialize / unserialize
author: tonivdv
layout: post
permalink: /2008/10/08/storing-multiple-data-in-1-database-field-with-php-serialize-unserialize/
dsq_thread_id:
  - 3828085558
categories:
  - database
  - mysql
  - php
tags:
  - database
  - php
  - serialize
---
While the serialize/unserialize functions exists since the early PHP 4 version, I ignored its existence. I discovered it recently doing some research on how to store multiple data in 1 database field.

Before continuing, keep in mind that it&#8217;s not always recommended to store multiple data in a database field, because it will be very difficult, if not impossible, to create queries using the inserted data. All serialized data will have to be unserialized, which takes time.

But why did I use it ?

The website I work on has a widget system (like netvibes and igoogle). Each widget has default settings that can be altered by a user. Those changes need to be saved in a database, so the next time the user loads the widget it will make use of the new settings.

When I first had to create this, I did not know of serialization in php, so I created the following db structure. (trying to respect DB Normalization).<figure id="attachment_164" style="width: 500px;" class="wp-caption aligncenter">

[<img class="size-full wp-image-164" title="widget and settings" src="http://www.devexp.eu/wp-content/uploads/2008/10/serialize-widgetsettings.png" alt="widget and settings" width="500" height="126" />][1]<figcaption class="wp-caption-text">widget and settings</figcaption></figure> 

You can see that I have a table with a list of widgets and a list of possible settings. Those two tables are connected to a widgetSetting table where the value of a setting to a widget is stored. It worked perfectly but it is not so proficient :

* To retrieve a widget&#8217;s settings, you need to join 2 tables.  
* To insert new widget settings, you need to check the settings table if a setting exists or not.  
* When updating widget settings, you need to check if a setting is still used or not.

This is a lot of work and database complexity for something that only is used for saving/retrieving settings of a widget. In my case it would be much better if I could save those settings in 1 field in the widget table.<figure id="attachment_165" style="width: 175px;" class="wp-caption aligncenter">

[<img class="size-full wp-image-165" title="widget" src="http://www.devexp.eu/wp-content/uploads/2008/10/serialize-widget.png" alt="widget" width="175" height="157" />][2]<figcaption class="wp-caption-text">widget</figcaption></figure> 

This is how I serialize my settings :

<pre class="brush: php; title: ; notranslate" title="">$settings = array(
'itemsOnPage' =&gt; 30,
'showChart' =&gt; true,
'startDate' =&gt; '01-10-2008',
'endDate' =&gt; '31-10-2008'
);

$serializedSettings = serialize($settings);

/**
* outputs:
*
* a:4:{s:11:"itemsOnPage";i:30;s:9:"showChart";b:1;s:9:"startDate";s:10:"01-10-2008";s:7:"endDate";s:10:"31-10-2008";}
*
*/</pre>

The serialize function will generate a storable representation of my array.  
Just save this string in a database field and the job is done. To rebuild the original array from this value you only need to call the unserialize function:

<pre class="brush: php; title: ; notranslate" title="">$storedRepresentation = 'a:4:{s:11:"itemsOnPage";i:30;s:9:"showChart";b:1;s:9:"startDate";s:10:"01-10-2008";s:7:"endDate";s:10:"31-10-2008";}';
$settings = unserialize($storedRepresentation);

/*
* print_r($settings) would give :
*
* Array
* (
*  [itemsOnPage] =&gt; 30
*  [showChart] =&gt; 1
*  [startDate] =&gt; 01-10-2008
*  [endDate] =&gt; 31-10-2008
* )
*/</pre>

As you can see the unserialize function correctly rebuild the original array.

It is also possible to serialize Objects with the exception of the built-in objects.

<pre class="brush: php; title: ; notranslate" title="">class MySettings {

private
$itemsOnPage = 30,
$showCart = true,
$startDate = '01-10-2008',
$endDate = '31-10-2008';

public function getItemsOnPage() {
return $this-&gt;itemsOnPage;
}
}

$mySettingsSerialized = serialize(new MySettings());

$mySettings = unserialize($mySettingsSerialized);

echo $mySettings-&gt;getItemsOnPage(); // 30</pre>

When serializing objects, PHP will attempt to call the member function \_\_sleep() prior to serialization. This is to allow the object to do any last minute clean-up, etc. prior to being serialized. Likewise, when the object is restored using unserialize() the \_\_wakeup() member function is called.

<pre class="brush: php; title: ; notranslate" title="">class MySettings {

private
$itemsOnPage = 30,
$showCart = true,
$startDate = '01-10-2008',
$endDate = '31-10-2008';

public function getItemsOnPage() {
return $this-&gt;itemsOnPage;
}

public function __wakeup() {
$this-&gt;itemsOnPage *= 10;
}
}

$mySettingsSerialized = serialize(new MySettings());

$mySettings = unserialize($mySettingsSerialized);

echo $mySettings-&gt;getItemsOnPage(); // 300</pre>

So you see it is very simple to use the seriailize/unserialize functions in php. But like I said in the beginning of the post, use it wisely when storing the serialized value in a database. I only use it when I do not need the value for creating sql queries.

 [1]: http://www.devexp.eu/wp-content/uploads/2008/10/serialize-widgetsettings.png
 [2]: http://www.devexp.eu/wp-content/uploads/2008/10/serialize-widget.png