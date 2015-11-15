---
title: 'Symfony Forms Framework: Change validators at runtime.'
author: Van de Voorde Toni
layout: post
permalink: /2009/02/15/symfony-forms-framework-change-validators-at-runtime/
dsq_thread_id:
  - 3828085554
categories:
  - Framework
  - php
  - Symfony
tags:
  - form
  - php
  - Symfony
---
Let’s say you have to make a user form with password fields. When you create the user you want the password fields to be **required**, but on an update the password fields should be **optional** because you don’t want the user the fill in his password on each update.

This is tricky because in the symfony forms Framework you have to predefine your validators. Your form would look like this:

<pre class="brush: php; title: ; notranslate" title="">class UserForm extends BaseUserForm {

  public function configure() {
    // Widgets
    $this-&gt;widgetSchema['password'] = new sfWidgetFormInputPassword();
    $this-&gt;widgetSchema['password_again'] = new sfWidgetFormInputPassword();

    // Validators
    $this-&gt;validatorSchema['password']-&gt;setOption('required', false);
    $this-&gt;validatorSchema['password_again'] = clone $this-&gt;validatorSchema['password'];

    $this-&gt;widgetSchema-&gt;moveField('password_again', 'after', 'password');

    $this-&gt;mergePostValidator(new sfValidatorSchemaCompare('password', sfValidatorSchemaCompare::EQUAL, 'password_again', array(), array('invalid' =&gt; 'The two passwords must be the same.')));
  }

}
</pre>

In this signature of the form I defined the password and password_again field as optional, which is correct for an update of the user, but incorrect for the creation of the user. How the hell can I change the validator of the password field when I create a user ?

Well thanks to the good and well designed framework of symfony it is very simple :).

<pre class="brush: php; title: ; notranslate" title="">class UserForm extends BaseUserForm {

  public function configure() {
    // Widgets
    $this-&gt;widgetSchema['password'] = new sfWidgetFormInputPassword();
    $this-&gt;widgetSchema['password_again'] = new sfWidgetFormInputPassword();

    // Validators
    $this-&gt;validatorSchema['password']-&gt;setOption('required', false);
    $this-&gt;validatorSchema['password_again'] = clone $this-&gt;validatorSchema['password'];

    $this-&gt;widgetSchema-&gt;moveField('password_again', 'after', 'password');

    $this-&gt;mergePostValidator(new sfValidatorSchemaCompare('password', sfValidatorSchemaCompare::EQUAL, 'password_again', array(), array('invalid' =&gt; 'The two passwords must be the same.')));
  }

  public function bind(array $taintedValues = null, array $taintedFiles = null) {

    if ( $this-&gt;object-&gt;isNew() ) {
      $this-&gt;validatorSchema['password']-&gt;setOption('required', true);
    }

    parent::bind($taintedValues, $taintedFiles);
  }

}
</pre>

What I did is override the function *bind* and before calling the bind method of the parent, I checked if the user object is new (means that we create a user) and if this is the case set the required option of the password field to true.