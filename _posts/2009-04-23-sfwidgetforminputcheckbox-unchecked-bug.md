---
title: sfWidgetFormInputCheckbox unchecked bug
author: tonivdv
layout: post
deprecated: true
permalink: /2009/04/23/sfwidgetforminputcheckbox-unchecked-bug/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085631
categories:
  - General
  - php
  - Symfony
tags:
  - form
  - php
  - sfWidgetFormInputCheckbox
  - Symfony
---
Ever tried to create a Symfony form with a check box input field, which by default should be unchecked ? No ? Well read on because it&#8217;s not that easy :).

This would be the code to print a check box input field in a Symfony form:

<pre class="brush: php; title: ; notranslate" title="">class CheckboxForm extends BaseCheckboxForm {
 public function configure() {
 $this-&gt;widgetSchema['field'] = new sfWidgetFormInputCheckbox();
 }
}
</pre>

When you use the form to create something then by default the check box should be unchecked. And when you use this form for editing, then the check box should be checked if the object you try to edit has already been checked in the past.

If you try this code, you&#8217;ll see that the check box will always be checked. Even if you display this form to create a new object. Strange no ?

<!--more-->

The problem is due to a bug in the Symfony sfWidgetFormInputCheckbox class. Even worse, this bug has been reported the first time 10 months ago (<a href="http://trac.symfony-project.org/ticket/3917" target="_blank">trac #3917</a>) and 1 month later (<a href="http://trac.symfony-project.org/ticket/3996" target="_blank">trac #3996</a>) and it&#8217;s still not fixed ! (grrrrr)

Until Symfony bug fixes this issue, there are 2 work a rounds you can use.

**1. Edit the sfWidgetFormInputCheckbox class and patch it:**

*actual*

<pre class="brush: php; title: ; notranslate" title="">class sfWidgetFormInputCheckbox extends sfWidgetFormInput
{
 ...
 public function render($name, $value = null, $attributes = array(), $errors = array())
 {
 if (!is_null($value) && $value !== false)
 {
 $attributes['checked'] = 'checked';
 }
 ...
 }
}
</pre>

*patched*

<pre class="brush: php; title: ; notranslate" title="">class sfWidgetFormInputCheckbox extends sfWidgetFormInput
{
 ...
 public function render($name, $value = null, $attributes = array(), $errors = array())
 {
 if (!is_null($value) && $value !== false && $value != 0)
 {
 $attributes['checked'] = 'checked';
 }
 ...
 }
}
</pre>

I would not recommend this, because if like us, you have more than one Symfony environment, there is a chance that you could forget to patch it. And worse if Symfony releases a new version without the bug fix you&#8217;ll have to remember to re patch the file.

**2. Extend the sfWidgetFormInputCheckbox class and override the render method:**

*Edit: Put this class in the lib folder and call it myOwnWidgetFormInputCheckbox.class.php. After that run symfony cc.*

<pre class="brush: php; title: ; notranslate" title="">/**
 * FIXME: This class can be removed if the sfWidgetFormInputCheckbox bug
 * has been resolved in symfony.
 */
class myOwnWidgetFormInputCheckbox extends sfWidgetFormInputCheckbox
{
 /**
 * Override render method due to symfony bug (http://trac.symfony-project.org/ticket/3917)
 */
 public function render($name, $value = null, $attributes = array(), $errors = array())
 {
 if (!is_null($value) && $value !== false && $value != 0)
 {
 $attributes['checked'] = 'checked';
 } 

 if (!isset($attributes['value']) && !is_null($this-&gt;getOption('value_attribute_value'))) {
 $attributes['value'] = $this-&gt;getOption('value_attribute_value');
 }

 return parent::render($name, null, $attributes, $errors);
 }
}
</pre>

Your form class should then be changed to:

<pre class="brush: php; title: ; notranslate" title="">class CheckboxForm extends BaseCheckboxForm {
 public function configure() {
 $this-&gt;widgetSchema['field'] = new myOwnWidgetFormInputCheckbox();
 }
}
</pre>

This was very easy to solve, but we lost a lot of time in finding the reason why this stupido check box was always checked. And why the hell is this bug still not solved in Symfony ?

In the mean time patch your symfony or create your own checkbox widget :).