---
title: Basic forms with Symfony
author: kennethvr
layout: post
deprecated: true
permalink: /2009/04/30/basic-forms-with-symfony/
categories:
  - php
  - Symfony
tags:
  - forms
  - module
  - Symfony
---
<img class="alignright size-full wp-image-727" title="Symfony" src="http://www.devexp.eu/wp-content/uploads/2009/04/symfony.jpg" alt="Symfony" width="127" height="35" />Our goal is to create a little crud that contains 4 fields

id (auto increment integer)  
name (varchar 50)  
value (double)  
type (0 or 1)

What I will explain is how to change your fields in the form after creation with generation of a module. So we start with a clean module that contains 1 action class and 3 templates (new, edit and index) and 1 partial (form) that is included on the new and edit template.  
We created this module not by hand but with the very easy command: symfony propel:generate-module.

<!--more-->So what we want to do is quite easy, but if you are not used to working with forms, this is already a big struggle. So go to the action and you will see that in the executeNew method a form is initialized with an instance of a class (in our case DiscountForm).

<pre class="brush: php; title: ; notranslate" title="">public function executeNew(sfWebRequest $request)
{
$this-&gt;form = new DiscountForm();
}
</pre>

If you now go to the page in your browser you will see that the form is build up for you in a very basic way, containing no layout and just the 4 labels, the input fields and a submit button. If you want the user to see error messages when he has forgotten to fill in a required field or if you want to add a field to the form you can do that in the DiscountForm by adding a function configure().

<pre class="brush: php; title: ; notranslate" title="">class DiscountForm extends BaseDiscountForm  {
public function configure() {
//insert extra data
}
}
</pre>

Now, as we don&#8217;t want the user to select 0 or 1 when choosing his type, we let him use a dropdown with understandable content to choose from. 0 is discount and 1 is up count. The purpose of the type is that the number inserted in the value field will be added or subtracted.

To do this we create a public $type array containing the discount and up count in the Discount class in the model. We do this in the model class because, in this way, we can re-use this in other methods later as we might need this in the templates or other classes.

<pre class="brush: php; title: ; notranslate" title="">static public $types = array(
'0' =&gt; 'Discount',
'1' =&gt; 'Up count',
);
</pre>

And in the configure method we put this:

<pre class="brush: php; title: ; notranslate" title="">$this-&gt;widgetSchema['type'] = new sfWidgetFormChoice(array(
'choices'  =&gt; Discount::$types,
'expanded' =&gt; true,
));
</pre>

If you reload your view on this point you will see that the type field has changed to a radio button field, which is easier to read.

You can style the form by adding some attributes to your fields:

<pre class="brush: php; title: ; notranslate" title="">&lt;?php echo $form['discount']-&gt;render(array('class' =&gt; 'page-input', 'size' =&gt; 5));  ?&gt;
</pre>

Page-input is the class specified in the css and giving the field a proper layout and size is the width of the field, in this case 5.  
All other attributes your element can accept can be inserted like this.

That&#8217;s actually it, if you followed these easy steps, you created your own basic crud. How nice it all looks will all depend on the css you use. For a few templates on forms you can take a look at <a href="http://www.smashingmagazine.com/2006/11/11/css-based-forms-modern-solutions/" target="_blank">this post </a>