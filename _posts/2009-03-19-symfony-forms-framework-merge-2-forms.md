---
title: 'Symfony Forms Framework: Merge 2 forms'
author: tonivdv
layout: post
deprecated: true
permalink: /2009/03/19/symfony-forms-framework-merge-2-forms/
dsq_thread_id:
  - 3828085562
categories:
  - Framework
  - Languages
  - php
  - Symfony
tags:
  - form
  - framework
  - php
  - Symfony
---
Recently I had to create a form to create/update users in our system. Some time ago we decided to save are users in 2 tables. The first table would contain all login information and the second his personal information. This is a simple example of the DB design:<figure id="attachment_463" style="width: 488px;" class="wp-caption aligncenter">

<img class="size-full wp-image-463" title="User db design" src="http://www.devexp.eu/wp-content/uploads/2009/03/dbdesign.png" alt="User db design" width="488" height="168" /><figcaption class="wp-caption-text">User db design</figcaption></figure> 

I would not recommend doing this for so little fields. But in our system we have a lot more fields, and it helps us to optimize our queries.

Wouldn&#8217;t it be better if we could merge the 2 forms? The answer is yes. And it&#8217;s pretty easy to do so in Symfony &#8230; too bad that it’s not documented on the Symfony website.

<!--more-->

When you generate the forms with Symfony, you’ll get a UserForm and a UserInfoForm.

<pre class="brush: php; title: ; notranslate" title="">/**
* User form.
*
* @package    form
*/
class UserForm extends BaseUserForm {

public function configure() {

}

}

/**
* UserInfo form.
*
* @package    form
*/
class UserInfoForm extends BaseUserInfoForm {

public function configure() {

}

}</pre>

The idea is to display one form to insert/update those 2 tables. If you are familiar to creating forms with Symfony you know that you could achieve it by instantiating the 2 forms in an action and send them to the templates:

<pre class="brush: php; title: ; notranslate" title="">/**
* Action Class
*/
class userActions extends sfActions
{
public function executeCreate($request) {
$this-&gt;userForm = new UserForm();
$this-&gt;userInfoForm = new UserInfoForm();
$this-&gt;setTemplate('edit');
}

public function executeEdit($request) {
$this-&gt;userForm = new UserForm(UserPeer::retrieveByPK($request-&gt;getParameter('id')));
$this-&gt;userInfoForm = new UserInfoForm(UserInfoPeer::retrieveByPK($request-&gt;getParameter('id')));
}

public function executeUpdate($request) {
$this-&gt;userForm = new UserForm(UserPeer::retrieveByPK($request-&gt;getParameter('id')));
$this-&gt;userInfoForm = new UserInfoForm(UserInfoPeer::retrieveByPK($request-&gt;getParameter('id')));

$this-&gt;userForm-&gt;bind($request-&gt;getParameter($this-&gt;userForm-&gt;getName()));
$this-&gt;userInfoForm-&gt;bind($request-&gt;getParameter($this-&gt;userInfoForm-&gt;getName()));

// etc ...
}
}

// Template code editSuccess.php
&lt;?php $user = $userForm-&gt;getObject(); ?&gt;

&lt;form action="&lt;?php echo url_for('user/update'.(!$user-&gt;isNew() ? '?id='.$user-&gt;getId() : '')) ?&gt;" method="post"&gt;
&lt;table&gt;
&lt;tfoot&gt;
&lt;tr&gt;
&lt;td colspan="2"&gt;
&lt;input type="submit" value="Save" /&gt;
&lt;/td&gt;
&lt;/tr&gt;
&lt;/tfoot&gt;
&lt;tbody&gt;
&lt;?php echo $userForm ?&gt;
&lt;?php echo $userInfoForm ?&gt;
&lt;/tbody&gt;
&lt;/table&gt;
&lt;/form&gt;
</pre>

This is really annoying to work this way. You always have to instance 2 forms in the action and the templates. Wouldn&#8217;t it be better if we could merge the 2 forms? The answer is yes. And it&#8217;s pretty easy to do so in Symfony &#8230; too bad that it’s not documented on the Symfony website.

Here is how you do it:

<span style="text-decoration: underline;"><strong>1. UserForm.class.php</strong></span>

<pre class="brush: php; title: ; notranslate" title="">/**
* User form.
*
* @package    form
*/
class UserForm extends BaseUserForm {

public function configure() {
$this-&gt;mergeForm(new UserInfoForm(UserInfoPeer::retrieveByPK($this-&gt;getObject()-&gt;getId())));
}

/**
* Override the save method to save the merged user info form.
*/
public function save($con = null) {
parent::save();

$this-&gt;updateUserInfo();

return $this-&gt;object;
}

/**
* Updates the user info merged form.
*/
protected function updateUserInfo() {
// update user info
if (!is_null($userInfo = $this-&gt;getUserInfo())) {

$values = $this-&gt;getValues();
if ( $userInfo-&gt;isNew() ) {
$values['user_id'] = $this-&gt;object-&gt;getId();
}

$userInfo-&gt;fromArray($values, BasePeer::TYPE_FIELDNAME);

$userInfo-&gt;save();
}
}

/**
* Returns the user info object. If it does
* not exist return a new instance.
*
* @return UserInfo
*/
protected function getUserInfo() {

if (!$this-&gt;object-&gt;getUserInfo()) {
return new UserInfo();
}

return $this-&gt;object-&gt;getUserInfo();
}
}
</pre>

<span style="text-decoration: underline;"><strong>2. actions.class.php</strong></span>

<pre class="brush: php; title: ; notranslate" title="">public function executeCreate($request) {
$this-&gt;form = new UserForm();
$this-&gt;setTemplate('edit');
}

public function executeEdit($request) {
$this-&gt;form = new UserForm(UserPeer::retrieveByPK($request-&gt;getParameter('id')));
}

public function executeUpdate($request) {

$this-&gt;forward404Unless($request-&gt;isMethod('post'));

$this-&gt;form = new UserForm(UserPeer::retrieveByPK($request-&gt;getParameter('id')));

$this-&gt;form-&gt;bind($request-&gt;getParameter($this-&gt;form-&gt;getName()));
if ($this-&gt;form-&gt;isValid()) {
$user = $this-&gt;form-&gt;save();
$this-&gt;redirect('user/edit?id='.$user-&gt;getId());
}

$this-&gt;setTemplate('edit');
}
</pre>

<span style="text-decoration: underline;"><strong>3. editSuccess.php</strong></span>

<pre class="brush: php; title: ; notranslate" title="">&lt;?php $user = $userForm-&gt;getObject(); ?&gt;

&lt;form action="&lt;?php echo url_for('user/update'.(!$user-&gt;isNew() ? '?id='.$user-&gt;getId() : '')) ?&gt;" method="post"&gt;
&lt;table&gt;
&lt;tfoot&gt;
&lt;tr&gt;
&lt;td colspan="2"&gt;
&lt;input type="submit" value="Save" /&gt;
&lt;/td&gt;
&lt;/tr&gt;
&lt;/tfoot&gt;
&lt;tbody&gt;
&lt;?php echo $userForm ?&gt;
&lt;/tbody&gt;
&lt;/table&gt;
&lt;/form&gt;
</pre>

As you can see, it’s the UserForm that will handle all the business of the UserInfoForm. This is great because the code in the action and template will be much more lightened and if needed it can easily be reused somewhere else.

This was a simple example on how to merge 2 forms, but since it&#8217;s not documented on the symfony website, it took me some time to fully understand on how to make it work. Now you can do much more advanced operations. 😉