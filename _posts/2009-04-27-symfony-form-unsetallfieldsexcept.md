---
title: 'Symfony Form: unsetAllFieldsExcept()'
author: tonivdv
layout: post
permalink: /2009/04/27/symfony-form-unsetallfieldsexcept/
dsq_thread_id:
  - 3828085646
categories:
  - General
  - php
  - Symfony
tags:
  - form
  - php
  - Symfony
---
When creating forms in symfony which extends from a Base (ORM) class, you sometimes do not need all fields that Propel/Doctrine generates for you. If you want to remove them you have several ways to do so.

**1. Unset the fiels you don&#8217;t need.**

<pre class="brush: php; title: ; notranslate" title="">public function configure() {
  unset($this['field1'], $this['field2'], ... );
}
</pre>

Simple way to remove the fields you don&#8217;t want, but if you change something in your database, which has an impact on that form (new column, foreignkey, etc ), it will be added in the (generated) super class. And if you don&#8217;t update the unset function with that extra field, it will be expected in the form. Unless you have unit tests for all your forms this could brake it without knowing it.

**2. Override the setup function.**

<pre class="brush: php; title: ; notranslate" title="">/**
 * @override
 */
public function setup()
  {
    parent::setup();

    $this-&gt;setWidgets(...);

    $this-&gt;setValidators(...);
  }
</pre>

Another way is to override the setup function and do your own widgets and validators initialization. This is a good way to only intialize the fields you really need, but you&#8217;ll have to do what your ORM generated for you.

**3. Use a function which unsets all fields except the one you need (like the enableAllPluginsExcept function).**

<pre class="brush: php; title: ; notranslate" title="">$this-&gt;unsetAllFieldsExcept(array(
      'field1',
      'field2',
      'field3',
      ...));
</pre>

This would be usefull, because it will remove all fields you do not want to use, and even if new fields are added it will not use them. The drawback is that if you have 20 fields and only need to unset one of them, you&#8217;ll have more type work ;).  
Since it does not exist in symfony, I quickly created our own function. This function should be added in the class <u>*BaseFormPropel.class*</u>:

<pre class="brush: php; title: ; notranslate" title="">abstract class BaseFormPropel extends sfFormPropel
{
  public function setup() {
  }

  protected function unsetAllFieldsExcept($fields = array()) {

    $unsetFields = array_diff(array_keys($this-&gt;getWidgetSchema()-&gt;getFields()), $fields);

    foreach($unsetFields as $value) {
      unset($this[$value]);
    }
  }
}
</pre>

**Which one to use ?**  
None of them are better. But if you need to reset a majority of the fields the ORM has generated then I would go for the method 2. If you only need to unset the fields you do not want to use then I would use the function unsetAllFieldsExcept().

Which method do/would you use ?