---
title: Select element, return 2 values
author: van Rumste Kenneth
layout: post
permalink: /2009/01/30/select-element-return-2-values/
dsq_thread_id:
  - 3828085555
categories:
  - html
  - Jquery
tags:
  - element
  - Jquery
  - option
  - return
  - select
  - values
---
I had a problem earlier today: I needed to have a select box that returns 2 or more values in stead of 1.  
I got this far:  
The value of the options gets the id of the item that is displayed in each option.  
I do have an other value that should be linked on this option. Is there an easy way to do this?

<pre class="brush: xml; title: ; notranslate" title="">&lt;select id="category" name="data[cat]"&gt; 
    &lt;option label="0" value="12"&gt;A&lt;/option&gt; 
    &lt;option label="0" value="7"&gt;B&lt;/option&gt; 
&lt;/select&gt;
</pre>

I tried to do this with entering the second value into a label attribute but then the problem is that i don&#8217;t know the correct way to get the label value of the selected option. This is written in Jquery.  
To get the selected option I use:

<pre class="brush: jscript; title: ; notranslate" title="">$("#category").livequery('change', function () {
var catId = ($(this)[0].value);
});
</pre>

the solution seems to be quite simple:

The label attribute is used by some browsers to display the content of the option, so your list box might display two 0 options.

A better solution might be to do the following:

<pre class="brush: xml; title: ; notranslate" title="">&lt;select id="category" name="data[cat]"&gt;
    &lt;option value="12,0"&gt;A&lt;/option&gt;
    &lt;option value="7,0"&gt;B&lt;/option&gt;
&lt;/select&gt;
</pre>

and split the value in the event handler, e.g.

<pre class="brush: jscript; title: ; notranslate" title="">$("#category").livequery('change', function () {            
    var values = ($(this)[0].value).split(",");
    var catId = values[0];
    var otherId = values[1];
});
</pre>

Wasn&#8217;t that easy