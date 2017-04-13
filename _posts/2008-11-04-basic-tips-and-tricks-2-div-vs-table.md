---
title: 'basic tips and tricks: 2. DIV vs. TABLE'
author: kennethvr
layout: post
permalink: /2008/11/04/basic-tips-and-tricks-2-div-vs-table/
syntaxhighlighter_encoded:
  - 1
categories:
  - css
  - html
  - Tips and Tricks
tags:
  - data
  - div
  - Layout
  - table
  - tag
---
A lot of people don&#8217;t really know when to use what kind of elements to build up an HTML page therefore I would like to take the time to explain tables and div elements.  
Tables are created to display data, they are built for it and that&#8217;s the only thing they are good for. To explain this we can take a look at the elements that a table can contain. Thead (again in tags <thead></thead>) contains the header row information of a table, tfoot contains the footer or bottom row of a table and tbody contains the body or data of a table. The easy thing about tables is that you can change them very easily without breaking out of the design, the height is fit automatically and data is displayed correctly the first time you show it. Off course with a little bit of CSS you get the layout just like you want it to. For more information on this you can take a look at my earlier post ([http://www.devexp.eu/?p=50][1])

Now what are div tags actually and why are they better to create a layout?

Div tags are nothing but logical divisions within the content of a page, they are built up with the <div> tag and make it easy to manage and style your HTML page just as you like. The cool thing about div tags is that you can name them individually, so you can affect them with CSS. As you can see the div will react a lot better with layout and give you all the possibilities you need.

The problem with tables is how unreadable and unusable they make your code. When you use a table as base to start from and create different rows and columns you will probably create other tables inside that table to display some content. And maybe you&#8217;ll end up with 3, 4 or even more tables within each other. When you have a page like that, please do take the time to look at the code and you&#8217;ll notice right away that it became very unreadable. When trying to change 1 column or 1 row you&#8217;ll see how complicated it gets. This method was very often used 3-4 years ago in web development but div tags are now getting the bigger hand in this now.

Let&#8217;s create a little example to illustrate this issue. This example will create data displayed like this:

<div style="padding: 5px; background-color: #f1f1f1; width: auto;">
  <table border="0" cellspacing="0" cellpadding="0" width="100%">
    <tr>
      <td colspan="2">
        This is a Title
      </td>
    </tr>
    
    <tr>
      <td width="50%">
        aaa
      </td>
      
      <td width="50%">
        bbb
      </td>
    </tr>
    
    <tr>
      <td colspan="2">
        cccc
      </td>
    </tr>
  </table>
</div>

<pre class="brush: xml; title: ; notranslate" title="">&lt;table border="0" cellspacing="0" cellpadding="0" width="100%"&gt;
&lt;tbody&gt;
&lt;tr&gt;
&lt;td colspan="2"&gt;This is a Title&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;td width="50%"&gt;aaa&lt;/td&gt;
&lt;td width="50%"&gt;bbb&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;td colspan="2"&gt;cccc&lt;/td&gt;
&lt;/tr&gt;
&lt;/tbody&gt;&lt;/table&gt;
</pre>

When we try to create this same view built up with div tags you will get:

<pre class="brush: xml; title: ; notranslate" title="">&lt;div style="width: 100%;"&gt;This is a Title&lt;/div&gt;
&lt;div style="width: 50%; float: left;"&gt;aaa&lt;/div&gt;
&lt;div style="width: 50%; float: right;"&gt;bbb&lt;/div&gt;
&lt;div style="width: 100%;"&gt;ccc&lt;/div&gt;
</pre>

I guess the example makes it pretty clear, you write less code and you are very flexible while using the div tags and it makes it very readable.

When creating pages, be sure to think about your design before starting and think of tables as data displayers, not as design tools. It will ask a lot of exercising when you&#8217;re not used to work with div tags, but after a while, you&#8217;ll see all their advantages. These tags and tables are built with a cause, tag for creating divisions in your page, tables for displaying data. Don&#8217;t mix them up.

 [1]: ../../../../../?p=50