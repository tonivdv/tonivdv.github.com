---
title: 'Basic tips and tricks: 4.Css'
author: kennethvr
layout: post
deprecated: true
permalink: /2009/09/01/basic-tips-and-tricks-4-css/
syntaxhighlighter_encoded:
  - 1
categories:
  - General
  - Tips and Tricks
tags:
  - class
  - css
  - objects
  - properties
  - tips
  - tricks
---
Hello everybody, I&#8217;m back with another version of basic tips and tricks.

After having a talk with a client, about the basic on css, I thought it might be good to have an easy introduction.

So let’s give it a try. We&#8217;ll talk about:

  * Why would you use css?
  * How to insert css in your html file
  * How css is written
  * Css on objects, classes and unique objects
  * Basic css properties and there values
  * The pseudo class
  * Where to find good information on css

> Cascading Style Sheets (CSS) is a style sheet language used to describe the presentation (that is, the look and formatting) of a document written in a markup language.
> 
> CSS is designed primarily to enable the separation of document content (written in HTML or a similar markup language) from document presentation, including elements such as the colors, fonts, and layout.

<!--more-->

**Why would you use css?**

Whenever you create an html page without css, you will probably insert layout into it. But every object will have to get it&#8217;s layout specifications and that could be a hard time, especially if you have a lot of objects that need the same layout.  
You don&#8217;t know any examples of objects with the same layout?

  * A menu with 4 or 5 buttons.
  * Buttons
  * A list
  * Different headings and titles
  * Paragraphs
  * &#8230;

As you can see, I don&#8217;t need to think very hard to get different objects on which the same layout can be applied.

I guess you see my point why you could use the same css code over and over again, instead of rewriting it for each object in your page.  
And I&#8217;m not even talking about different pages or a whole website&#8230;

**How to insert css in your html file**

CSS is a file containing code you and that puts a layout on specific objects in your html file.

That code can be written in a html file within these tags

<pre class="brush: xml; title: ; notranslate" title="">&lt;STYLE type=text/css&gt;                 &lt;/STYLE&gt;
</pre>

Or in a css file that is included within the <head></head> tags.

Whatever css code you write in there will be applied to the code within that page.

If you are creating a website or more than one page, it might be more sufficient to use an include so you can reuse the same code over and over again.

In my opinion the only times you can or should use the <style> tags without an include is if you are sure you will never create another page with the same layout or for testing.

Just for readability alone, it might be better to separate it from your original html file and include it.

**How css is written**

The coding is quite simple, the only difficult thing to learn is: what properties are there? And on which object are they possible?

Css is always written in this way:

<pre class="brush: css; title: ; notranslate" title="">selector [, selector2, ...]:pseudo-class { property: value; }
/* comment*/
</pre>

This means:

Selector  
Is the object you are trying to set the layout for.  It&#8217;s possible to put more than one selector by separating them with a comma.  
Ex: paragraphs, header, etc&#8230;

Pseudo class  
The pseudo class is a bit tricky and I will talk about that at the end of my tutorial.  
Ex: hover, link, visited, etc&#8230;

Property  
This is the property you want to set for a specific selector.  
Ex: background-color, font-size, etc&#8230;

Value  
This is the value you set for the property.  
Ex: 1px, #f1f1f1, none, etc&#8230;

Comment  
Comment is written between /\* \*/ tags and can be 1 or more lines in length.

**Css on objects, classes and unique objects**

The selector is actually more then only an id and can be defined in various ways. I guess the best way to show this is by using some examples.  
This will set all paragraphs in your page with the background color red.

<pre class="brush: css; title: ; notranslate" title="">p { background-color: red;}
&lt;p&gt;blabla&lt;/p&gt;
</pre>

This will set all paragraphs with the class nature with a background green.

<pre class="brush: css; title: ; notranslate" title="">p.nature { background-color: green; }
&lt;p class="nature"&gt;blabla&lt;/p&gt;
</pre>

This will put a background color green on every paragraph and a brow background color on every font with class tree within the nature paragraph.

<pre class="brush: css; title: ; notranslate" title="">p.nature { background-color: green; }
p.nature font.tree { background: brown; }
&lt;p class="nature"&gt; blabla &lt;font class="tree"&gt; more bla &lt;/font&gt;
&lt;/p&gt;
</pre>

If you are willing to put a layout on 1 specific object you can do that by using the #. In this case, the paragraph sky will get a blue background.

<pre class="brush: css; title: ; notranslate" title="">#sky { background-color: blue}
&lt;p id="sky"&gt;blabla&lt;/p&gt;
</pre>

This part is very important because you can save tons of time and code by using classes and ids.

**Basic css properties and there values**

There are quite a few different properties and not all properties can be used on any object.

It&#8217;s not very complex, but with some try and error you&#8217;ll find your way quite fast in the exciting world of properties. <img src="http://www.devexp.eu/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

I&#8217;ll give some examples first from simple to a bit more difficult and there possible values.

<pre class="brush: css; title: ; notranslate" title="">background-color: #cccccc;
/* #cccccc is a grey color and can also be replaced by the string 'grey' */

margin: 5px;
/* this will put a margin of 5 pixels round the element */

font-size: normal;
/* The font size can also be replaced with number of px,em or % */

background: #FFFFFF url(image.jpg) repeat-x scroll left top;
/* As you can see it is also possible to add more than 1 value to some properties. */

#FFFFFF:          /* Is the default color the background has */
url(image.jpg)    /* is the image that will be displayed on the background*/
repeat-x          /*  is the way it will be repeated in. x is horizontal, y is vertical*/
scroll            /* sets if the image will be fixed or scroll with the rest of the page*/
left top          /* is where the background starts*/

/* Those are the more complex properties but can easily split up into different css properties if it's too complex.*/
background-color:  #FFFFFF;
background-image:  url(image.jpg);
background-repeat:  repeat-x;
background-attachment: scroll;
background-position: top left;
</pre>

More properties are possible and you can find them other websites that I got lined up in the last part: &#8216;where to find good information on css&#8217;

<strong>The pseudo class</strong>

These are used to add special effects to the object they are put on.

In this example you will add different text colors when the link is visited, hovered or active

<pre class="brush: css; title: ; notranslate" title="">a:link {color:#FF0000}     /* unvisited link */
a:visited {color:#00FF00}  /* visited link */
a:hover {color:#FF00FF}    /* mouse over link */
a:active {color:#0000FF}   /* selected link */
</pre>

Other pseudo-classes are possible like: first-child, focus, lang, etc&#8230;

**Where to find good information on css**

<a href="http://en.wikipedia.org/wiki/Cascading_Style_Sheets" target="_blank">General information </a>  
<a href="http://jigsaw.w3.org/css-validator/" target="_blank">Test your css</a>  
<a href="http://www.w3.org/Style/CSS/" target="_blank">Official w3c website </a>

Off course this doesn&#8217;t cover the whole story as this is only a basic guide.

I just hope you have some advantage from reading this.

C u next time. K.